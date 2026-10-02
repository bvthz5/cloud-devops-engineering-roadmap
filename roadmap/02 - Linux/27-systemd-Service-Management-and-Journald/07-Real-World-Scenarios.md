# 07 — Real-World systemd Production Scenarios

Practical operational challenges encountered when managing production systemd services.

---

## Scenario 1: Mitigating Microservice OOM Cascades

### Incident Summary
A Java microservice with a memory leak consumes 95% of host RAM. Linux kernel OOM killer activates and kills `sshd` and `dockerd`, taking the entire node offline.

### Root Cause Analysis
The service was deployed without cgroup limits, competing in the root slice with critical system infrastructure processes.

### Production Solution
Isolate the service with strict cgroup limits and an OOM score adjustment:
```ini
[Service]
MemoryMax=4G
MemoryHigh=3.5G
# Protect systemd and sshd by setting higher OOM score on the application
OOMScoreAdjust=500
```
When memory exceeds 4GB, the kernel kills **only the application process**, and systemd automatically restarts it cleanly via `Restart=always` without affecting host stability.

---

## Scenario 2: Zero-Downtime Hot Configuration Reload

### Incident Summary
Updating production NGINX or Envoy configuration requires applying changes without dropping in-flight client TCP connections.

### Production Solution
Use `ExecReload=` with POSIX signal delivery (`SIGHUP`):
```ini
[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
```
When running `sudo systemctl reload nginx`:
1. Master process reads new config.
2. Spawns new worker processes with new config.
3. Gracefully tells old worker processes to finish active requests and shut down.
4. **Zero dropped connections!**

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - cgroups v2 Resource Limits](./06-cgroups-v2-Resource-Limits-in-systemd.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
