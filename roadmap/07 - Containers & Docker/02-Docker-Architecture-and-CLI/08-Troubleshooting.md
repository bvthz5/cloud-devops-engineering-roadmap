# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Diagnosing Docker Daemon Hangs

```bash
# Check systemd status
systemctl status docker

# Dump stack traces of all running daemon goroutines (SIGUSR1)
kill -USR1 $(pidof dockerd)
# Inspect traces in: journalctl -u docker -n 200

# Verify Docker daemon Unix socket permissions
ls -la /var/run/docker.sock
# Expected: srw-rw---- 1 root docker
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
