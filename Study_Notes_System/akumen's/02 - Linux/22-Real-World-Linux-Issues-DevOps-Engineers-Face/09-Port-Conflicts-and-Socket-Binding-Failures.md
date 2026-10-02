# 09 — Port Conflicts and Socket Binding Failures

## 1. Scenario
Attempting to start Nginx or Docker container fails with:
`bind() to 0.0.0.0:80 failed (98: Address already in use)`

```text
Scenario
   ↓
Symptoms: "Address already in use", EADDRINUSE error logs, service fails to bind socket
   ↓
What could cause it? Pre-existing daemon (e.g. Apache) or zombie process binding to port 80
   ↓
Diagnostic commands: ss -tulpn | grep :80, lsof -i :80, fuser 80/tcp
   ↓
Find root cause: Apache process (PID 3102) currently listening on port 80
   ↓
Fix / mitigate: Stop conflicting service (systemctl stop apache2) or kill process (fuser -k 80/tcp)
```

## 2. Diagnostic Commands & Socket Inspection

```bash
# 1. List listening TCP/UDP sockets with process PIDs
ss -tulpn | grep ':80'

# 2. Alternative lsof socket lookup
lsof -i :80

# 3. Check process holding target port using fuser
fuser 80/tcp
```

### Sample `ss -tulpn` Output Analysis
```text
Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port  Process
tcp    LISTEN  0       511      0.0.0.0:80           0.0.0.0:*          users:(("apache2",pid=3102,fd=4))
```
- **`pid=3102`**: Process ID binding port 80 is `3102` (`apache2`).

## 3. Resolution Steps

```bash
# Gracefully stop conflicting service:
systemctl stop apache2

# If rogue orphaned process, kill forcibly:
fuser -k -9 80/tcp
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Systemd Service Failures](./08-Systemd-Service-Failures-and-Crash-Loops.md) | [README](./README.md) | [10 - SSH Connectivity & Auth Debugging](./10-SSH-Connectivity-Auth-and-Config-Troubleshooting.md) |
