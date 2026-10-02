# Containers & Docker Interview Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Docker Container in CrashLoopBackOff with Exit Code 137 (OOMKilled)

### 🚨 The Production Scenario
A Node.js microservice running in Docker/Kubernetes restarts every 15 minutes with Exit Code 137 under moderate customer traffic.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Exit Code 137 is `128 + 9` (`SIGKILL`), indicating the Linux kernel OOM killer or Docker daemon terminated the container because its resident memory (RSS) exceeded the configured memory cgroup limit (`memory.max`). Common root causes include Node.js V8 heap growth exceeding the container's hard limit, unclosed database connection pools, or in-memory file buffering.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Inspect container termination metadata using `docker inspect <container> | grep -i oom` or `kubectl describe pod`.
- Step 2: Confirm `OOMKilled: true` and verify memory limit thresholds.
- Step 3: Profile container memory consumption in real time using `docker stats`.
- Step 4: Configure runtime flags (e.g. `--max-old-space-size` for Node.js) to trigger garbage collection before the cgroup limit.
- Step 5: Right-size container memory requests and limits based on actual p99 heap usage.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify if container terminated due to OOM kill condition
docker inspect <CONTAINER_ID> --format '{{.State.ExitCode}} OOMKilled: {{.State.OOMKilled}}'

# Inspect real-time memory usage, cache buffers, and limit percentage per container
docker stats --no-stream

# Inspect K8s last terminated state reason and exit code
kubectl get pod <POD> -o jsonpath='{.status.containerStatuses[*].lastState.terminated}'

# Check cgroup memory allocation failure counters (cgroups v1/v2)
cat /sys/fs/cgroup/memory/docker/<ID>/memory.failcnt

# Run container with strict memory limit and disabled swap expansion
docker run -m 1g --memory-swap 1g myapp:latest

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Exit code 137 is a fatal SIGKILL issued by the Linux kernel cgroup subsystem when a container exceeds its memory limit. I confirm this via `docker inspect` checking `State.OOMKilled: true`. To fix it, I ensure the application runtime's internal heap limit (like `--max-old-space-size` for Node or `-XX:MaxRAMPercentage` for Java) is tuned to roughly 75% of the container's cgroup memory limit, leaving 25% for OS buffers and native thread stacks."

---

## 📌 Scenario 2: Container Disk Leak Filling Root Partition on Host Server

### 🚨 The Production Scenario
A production Docker host's root disk (`/`) reaches 100% capacity. `df -h` shows `/var/lib/docker/overlay2` consuming 200GB.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
By default, Docker's `json-file` logging driver stores stdout/stderr output uncompressed indefinitely unless maximum log size and file rotation are configured. Additionally, applications writing temporary files inside the container's writable layer (rather than an ephemeral volume or `tmpfs`) prevent disk blocks from being freed.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Identify space hogs in Docker root directory using `docker system df -v`.
- Step 2: Check for oversized `*-json.log` files in `/var/lib/docker/containers/*`.
- Step 3: Safely truncate bloated logs without restarting containers.
- Step 4: Configure `/etc/docker/daemon.json` with log rotation (`max-size: 50m`, `max-file: 3`).
- Step 5: Run `docker system prune --volumes` during a scheduled maintenance window to reclaim orphaned layers.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Display comprehensive breakdown of space used by images, containers, local volumes, and build cache
docker system df -v

# List all container JSON log files sorted by physical disk footprint
find /var/lib/docker/containers/ -name '*-json.log' -exec ls -lh {} \;

# Instantly truncate container log files to zero bytes without restarting services
truncate -s 0 /var/lib/docker/containers/*/*-json.log

# Remove all unused images, dangling volumes, and stopped containers
docker system prune -af --volumes

# Configure global automated log rotation in Docker daemon
cat <<EOF > /etc/docker/daemon.json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "50m",
    "max-file": "3"
  }
}
EOF

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Disk exhaustion under `/var/lib/docker` is usually caused by unrotated JSON container logs or unbounded writable overlay2 container layers. I immediately reclaim space by zeroing active log files via `truncate -s 0`. The permanent architectural solution is enforcing global log rotation in `/etc/docker/daemon.json` with `max-size: 50m` and `max-file: 3`, and mounting high-throughput temporary write directories to in-memory `tmpfs` mounts instead of the container rootfs."

---

## 📌 Scenario 3: Docker Image Build Time Surging to 40 Minutes in CI/CD Pipeline

### 🚨 The Production Scenario
A Python/React monorepo Docker build in a GitHub Actions pipeline takes 40 minutes on every commit, severely delaying software delivery.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The `Dockerfile` invalidates the layer cache early by running `COPY . .` before installing dependencies (`pip install` or `npm install`). Any code or README change forces Docker to rebuild all subsequent dependency layers from scratch. In addition, multi-stage builds and buildkit cache exports are absent.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Reorder `Dockerfile` instructions: copy dependency manifests (`package.json`, `requirements.txt`) first.
- Step 2: Add comprehensive `.dockerignore` to exclude `.git`, `node_modules`, `tests`, and local artifacts.
- Step 3: Enable Docker BuildKit (`DOCKER_BUILDKIT=1`).
- Step 4: Implement multi-stage builds separating build tools/compilers from runtime environments.
- Step 5: Utilize remote registry caching (`--cache-from` / `--cache-to`) across CI workers.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Enable BuildKit with inline cache metadata for distributed CI reuse
DOCKER_BUILDKIT=1 docker build --build-arg BUILDKIT_INLINE_CACHE=1 -t myapp:latest .

# Export and import layer cache from container registry across ephemeral CI runners
docker buildx build --cache-to type=registry,ref=myrepo/app:cache,mode=max --cache-from type=registry,ref=myrepo/app:cache -t myrepo/app:v1 .

# Audit .dockerignore to ensure .git and temporary files do not bust cache
cat .dockerignore

# Analyze image layer sizes and cache efficiency using Dive tool
dive myapp:latest

# Inspect build layer breakdown, commands, and size footprints
docker history myapp:latest

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Sluggish Docker builds stem from poor layer caching and unnecessary context transfer. I fix this by restructuring the Dockerfile so that static dependency definitions (`package.json`, `requirements.txt`) are copied and installed before copying application code. I add a strict `.dockerignore`, adopt multi-stage builds with a minimal Distroless/Alpine runtime, and use `docker buildx` with remote registry caching (`--cache-from`) so ephemeral CI runners share cached layers."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Git & Version Control Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../03-Git-and-Version-Control-Interview-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Containers & Docker Interview Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

