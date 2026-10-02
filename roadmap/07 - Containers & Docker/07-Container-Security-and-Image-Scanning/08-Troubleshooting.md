# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Debugging "Read-Only File System" Errors

When running containers with `--read-only`, applications that expect to write temporary files, PIDs, or logs will fail:

```bash
# Identify where the app is trying to write
docker logs hardened_container
# Output: [emerg] open() "/var/run/nginx.pid" failed (30: Read-only file system)

# Solution: Mount specific writable tmpfs locations
docker run -d \
  --read-only \
  --tmpfs /var/run:rw,noexec \
  --tmpfs /var/cache/nginx:rw,noexec \
  nginx:alpine
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
