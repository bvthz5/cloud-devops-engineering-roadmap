# Containers & Docker Interview Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Critical CVE in Base Image Blocking Production Release

### 🚨 The Production Scenario
A high-severity remote code execution vulnerability (CVSS 9.8) is flagged by Trivy in `node:18-bullseye`. Security policy blocks deployment.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Standard full OS base images (`ubuntu`, `debian`, `bullseye`) package hundreds of unnecessary binaries (curl, perl, bash, package managers) that inflate the attack surface and generate frequent CVE alerts.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Scan image to identify affected packages: `trivy image myapp:latest`.
- Step 2: Migrate the Dockerfile to a minimal base image (Alpine, Distroless, or Chainguard).
- Step 3: Use multi-stage builds: compile in build image, copy only artifacts to production runtime.
- Step 4: Remove package managers (`apt-get`, `apk`) from the final production runtime.
- Step 5: Verify new scan reports 0 critical and 0 high vulnerabilities.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Scan container image for critical vulnerabilities and print CVE details
trivy image --severity HIGH,CRITICAL myapp:latest

# Alternative enterprise vulnerability scanner validating against CVE databases
grype myapp:latest --fail-on critical

# Build streamlined image based on Google Distroless runtime
docker build -t myapp:distroless -f Dockerfile.distroless .

# Verify zero critical CVEs remain in optimized minimal container
trivy image --severity CRITICAL myapp:distroless

# Inspect container filesystem contents without running the container
crane export myapp:latest - | tar -tvf - | grep -i vulnerable_pkg

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The most effective way to eliminate base image CVEs is reducing the attack surface to zero unnecessary binaries. I refactor the Dockerfile using multi-stage builds: the first stage handles compilation with standard SDKs, and the final production stage copies only the compiled binary into a Google Distroless or Chainguard minimal image containing no shell or package manager. This eliminates 95% of CVEs and prevents attack pivoting."

---

## 📌 Scenario 8: Docker Daemon Unresponsive and 'docker ps' Hanging

### 🚨 The Production Scenario
Production engineers report that `docker ps` and container orchestrators hang indefinitely on a node with 100 containers.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Docker daemon hangs typically occur due to containerd lock contention, storage driver deadlocks (overlay2 on slow disks), or unclosed kernel sockets during intense I/O. If containerd becomes unresponsive, the Docker daemon's UNIX socket `/var/run/docker.sock` blocks incoming CLI requests.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check daemon and containerd status with `systemctl status docker containerd`.
- Step 2: Inspect `/var/log/docker.log` or `journalctl -u docker -u containerd -n 100`.
- Step 3: Send `SIGUSR1` to the dockerd process to dump goroutine stack traces without restarting.
- Step 4: Check if Docker 'live-restore' is enabled (`"live-restore": true`), which allows containers to keep running even if the daemon restarts.
- Step 5: Restart the Docker daemon safely: `systemctl restart docker`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Trigger Docker daemon goroutine stack dump into systemd journal for deadlock analysis
kill -s USR1 $(pidof dockerd)

# Inspect Docker daemon stack traces and identify stuck mutexes
journalctl -u docker -n 50 --no-pager

# Verify live-restore is enabled to keep running containers alive during daemon restarts
grep -i 'live-restore' /etc/docker/daemon.json

# Safely restart container runtime services
systemctl restart containerd && systemctl restart docker

# Confirm Docker socket connectivity and healthy daemon response
docker info

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When `docker ps` hangs, it indicates deadlocks in dockerd or containerd, often around filesystem I/O locks. I send `SIGUSR1` to dockerd to dump all active goroutine stacks into `journalctl` for root-cause analysis. Crucially, I always verify that `"live-restore": true` is enabled in `/etc/docker/daemon.json` before restarting dockerd, ensuring running production containers suffer zero downtime when the daemon restarts."

---

## 📌 Scenario 9: Multi-Architecture Docker Builds (x86_64 vs ARM64) Failing in CI

### 🚨 The Production Scenario
An organization adopts AWS Graviton (ARM64) instances for cost savings, but container images built on x86_64 CI runners fail on EC2 with `exec /app: exec format error`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Linux binaries compiled for x86_64 CPU instruction sets cannot execute on ARM64 processors without CPU emulation. When an image is built on standard x86 CI workers without multi-arch tooling, it pushes only an x86_64 manifest.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Identify current image architecture using `docker inspect` or `crane manifest`.
- Step 2: Install QEMU user-mode emulators on build runners: `docker run --privileged --rm tonistiigi/binfmt --install all`.
- Step 3: Create a Docker Buildx builder instance with multi-platform drivers.
- Step 4: Execute `docker buildx build --platform linux/amd64,linux/arm64 -t myrepo/app:latest --push .`.
- Step 5: Verify that the registry receives a multi-arch manifest list.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Create and activate a new Buildx builder supporting cross-compilation
docker buildx create --name multiarch-builder --use

# Verify supported platforms (linux/amd64, linux/arm64, linux/riscv64)
docker buildx inspect --bootstrap

# Build and push multi-architecture image manifest in a single command
docker buildx build --platform linux/amd64,linux/arm64 -t myrepo/app:v1 --push .

# Inspect remote registry manifest to confirm presence of both amd64 and arm64 architectures
docker manifest inspect myrepo/app:v1

# Locally test running ARM64 container under QEMU emulation
docker run --rm --platform linux/arm64 myrepo/app:v1 uname -m

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "`exec format error` means an architecture mismatch — running an x86_64 binary on an ARM64 CPU. I solve this by standardizing our CI pipelines on `docker buildx` with multi-platform targets (`--platform linux/amd64,linux/arm64`). This generates an OCI Manifest List in the registry, allowing AWS Graviton nodes to pull the ARM64 image while local developer laptops pull AMD64 seamlessly."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Containers & Docker Interview Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Containers & Docker Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

