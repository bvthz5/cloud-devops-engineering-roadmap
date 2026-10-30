# Containers & Docker Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Docker Compose Dependency Race Conditions on Startup

### 🚨 The Production Scenario
A microservices stack running in Docker Compose crashes on startup because the backend application tries to connect to PostgreSQL before the database has finished initializing.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Docker Compose `depends_on` by default only waits for the dependency container to start (`running` state), not for the application inside (e.g. PostgreSQL) to be healthy and accepting network connections.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Add a proper `healthcheck` block to the PostgreSQL service in `docker-compose.yml` (`test: ["CMD-SHELL", "pg_isready -U postgres"]`).
- Step 2: Update application service `depends_on` to use `condition: service_healthy`.
- Step 3: Implement connection retry loops with exponential backoff in the application code.
- Step 4: Use lightweight entrypoint wait scripts (e.g. `wait-for-it.sh` or `dockerize`).
- Step 5: Test stack startup with `docker compose up --abort-on-container-exit`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect status and health check state of all services in stack
docker compose ps

# Monitor database initialization and ready state messages
docker compose logs -f db

# View detailed health check command output and failure count
docker inspect --format='{{json .State.Health}}' <DB_CONTAINER_ID>

# Start Compose stack and block until all services report healthy status
docker compose up -d --wait

# Tear down stack and wipe volumes to verify clean startup from scratch
docker compose down -v

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Standard `depends_on` only verifies container startup, not application readiness. In `docker-compose.yml`, I define a robust `healthcheck` using `pg_isready` on the database service, and configure the application's `depends_on` with `condition: service_healthy`. In application code, I always implement connection retries with exponential backoff so microservices gracefully survive brief database restarts."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Docker Container in CrashLoopBackOff with Exit Code 137 (OOMKilled)** | `docker inspect <CONTAINER_ID> --format '{{.State.ExitCode}} OOMKilled: {{.State.OOMKilled}}'` | Exit Code 137 is `128 + 9` (`SIGKILL`), indicating the Linux kernel OOM killer o... |
| **Scenario 2: Container Disk Leak Filling Root Partition on Host Server** | `docker system df -v` | By default, Docker's `json-file` logging driver stores stdout/stderr output unco... |
| **Scenario 3: Docker Image Build Time Surging to 40 Minutes in CI/CD Pipeline** | `DOCKER_BUILDKIT=1 docker build --build-arg BUILDKIT_INLINE_CACHE=1 -t myapp:latest .` | The `Dockerfile` invalidates the layer cache early by running `COPY . .` before ... |
| **Scenario 4: Zombie Processes Accumulating Inside Containers Causing Failure** | `docker top <CONTAINER_ID> -ef` | When a container starts, the application entrypoint runs as PID 1 inside the con... |
| **Scenario 5: Rootless Container Cannot Bind to Privileged Ports 80 and 443** | `sed -i 's/listen 80;/listen 8080;/g' /etc/nginx/conf.d/default.conf` | In Linux, ports below 1024 are privileged ports restricted to the root user (`UI... |
| **Scenario 6: Container Cannot Resolve External DNS or Cloud Metadata Service** | `sysctl net.ipv4.ip_forward` | Docker daemon creates a bridge network (`docker0`) with iptables NAT rules (`MAS... |
| **Scenario 7: Critical CVE in Base Image Blocking Production Release** | `trivy image --severity HIGH,CRITICAL myapp:latest` | Standard full OS base images (`ubuntu`, `debian`, `bullseye`) package hundreds o... |
| **Scenario 8: Docker Daemon Unresponsive and 'docker ps' Hanging** | `kill -s USR1 $(pidof dockerd)` | Docker daemon hangs typically occur due to containerd lock contention, storage d... |
| **Scenario 9: Multi-Architecture Docker Builds (x86_64 vs ARM64) Failing in CI** | `docker buildx create --name multiarch-builder --use` | Linux binaries compiled for x86_64 CPU instruction sets cannot execute on ARM64 ... |
| **Scenario 10: Docker Compose Dependency Race Conditions on Startup** | `docker compose ps` | Docker Compose `depends_on` by default only waits for the dependency container t... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Containers & Docker Interview Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Kubernetes & Orchestration Interview Scenarios: Production Incidents & Triage Scenarios →](../05-Kubernetes-and-Orchestration-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

