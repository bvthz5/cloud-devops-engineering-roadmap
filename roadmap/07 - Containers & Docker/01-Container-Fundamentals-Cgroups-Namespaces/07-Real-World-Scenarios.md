# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Fork Bomb Kubernetes Node Failure (`pids.max`)

### Context & Incident
A developer deployed a test Python microservice to a multi-tenant Kubernetes cluster. The service had a recursion bug that spawned child processes in an infinite loop. Within 30 seconds, the entire 64-core physical Kubernetes node became unresponsive; `kubelet` stopped reporting, SSH timed out, and all 40 adjacent pods running on the node crashed.

### Root Cause
While CPU and memory limits were configured, the PID controller (`pids.max`) was left unset. The single rogue container spawned 32,768 processes, exhausting the host kernel's PID table (`/proc/sys/kernel/pid_max`) and preventing the host OS from spawning any new processes (including SSH and monitoring daemons).

### Architectural Solution: Enforce `pids-limit`
In Docker:
```bash
docker run --pids-limit 100 myapp
```
In Kubernetes (`kubelet` configuration):
```yaml
podPidsLimit: 512
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Container Security Boundaries](./06-Container-Security-Boundaries-and-Kernel-Surface.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
