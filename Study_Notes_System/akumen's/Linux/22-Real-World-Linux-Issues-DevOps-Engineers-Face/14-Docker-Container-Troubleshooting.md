# 14 — Docker Container Troubleshooting

## 1. Scenario
A Docker container terminates immediately after launch with `State: Exited (137)`.

```text
Scenario
   ↓
Symptoms: Container exited with status 137, OOMKilled true in docker inspect
   ↓
What could cause it? Container exceeded memory limit configured in docker run / docker-compose
   ↓
Diagnostic commands: docker ps -a, docker inspect container_id, docker logs container_id
```

## 2. Diagnosing Exit Codes

| Exit Code | Meaning | Common Cause |
|---|---|---|
| `137` | `SIGKILL` (128 + 9) | OOM Killed by host kernel or `docker --memory` limit. |
| `139` | `SIGSEGV` (128 + 11) | Segmentation fault / memory corruption in container binary. |
| `1` / `255` | General application error | Missing env var, unhandled promise rejection, syntax error. |

## 3. Essential Inspection Commands

```bash
# Check container status and exit code
docker ps -a --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Inspect detailed JSON metadata (check OOMKilled state)
docker inspect --format='{{.State.OOMKilled}}' container_name

# Read container stdout/stderr log output
docker logs --tail 100 -f container_name
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - Deployment Failures & Rollback](./13-Production-Deployment-Failures-and-Rollback.md) | [README](./README.md) | [15 - Kubernetes Pod CrashLoopBackOff](./15-Kubernetes-Pod-CrashLoopBackOff-Troubleshooting.md) |
