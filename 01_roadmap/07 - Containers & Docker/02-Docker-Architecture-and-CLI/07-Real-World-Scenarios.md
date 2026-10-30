# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Runaway Log Disk Exhaustion Outage

### Context & Incident
A production Kubernetes worker node running Docker Engine froze, transitioned to `NotReady`, and evicted all running pods. Node disk space reported 100% full (`/var/lib/docker`).

### Root Cause
Docker's default logging driver (`json-file`) has **no size limits or rotation** by default! A Java container crashed into a tight loop printing stack traces, generating a single 95GB JSON log file under `/var/lib/docker/containers/<id>/<id>-json.log`.

### Prevention
Configure global log rotation in `/etc/docker/daemon.json`:
```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "50m",
    "max-file": "3"
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Multi Architecture Builds with Buildx](./06-Multi-Architecture-Builds-with-Buildx.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
