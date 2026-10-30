# 05 - Daemon Configuration and Production Tuning

## 1. Hardened Production `/etc/docker/daemon.json`

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "20m",
    "max-file": "5"
  },
  "live-restore": true,
  "storage-driver": "overlay2",
  "default-ulimits": {
    "nofile": {
      "Name": "nofile",
      "Hard": 65535,
      "Soft": 65535
    }
  },
  "no-new-privileges": true,
  "userland-proxy": false,
  "metrics-addr": "127.0.0.1:9323",
  "experimental": true
}
```

### Key Production Flags Explained:
- **`live-restore: true`**: Keeps containers running when the Docker daemon is restarted or updated.
- **`log-opts`**: Enforces strict automatic log rotation, preventing runaway logs from filling root filesystems!
- **`userland-proxy: false`**: Disables the inefficient user-space `docker-proxy` process; routes traffic entirely via high-speed kernel `iptables` NAT.
- **`no-new-privileges: true`**: Prevents container processes from gaining additional privileges via `setuid` binaries.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Resource Constraints and Limits](./04-Resource-Constraints-and-Limits.md) | [Index](../../../README.md) | [06 - Multi Architecture Builds with Buildx →](./06-Multi-Architecture-Builds-with-Buildx.md) |
