# 06 - Storage Optimization and Dangling Cleanup

## 1. Reclaiming Orphaned Volume Space

When a container created with anonymous volumes is removed with `docker rm` (without the `-v` flag), the anonymous volume remains on disk permanently (**Dangling Volume**):

```bash
# Identify dangling (unattached) volumes
docker volume ls -f dangling=true

# Purge all dangling volumes
docker volume prune -f

# Check overall disk space consumed by volumes
docker system df -v
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - External Volume Plugins](./05-External-Volume-Plugins-and-Cloud-Storage.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
