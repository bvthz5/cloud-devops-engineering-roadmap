# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Named Volume
docker run -d --mount type=volume,src=myvol,dst=/data app

# Read-only Bind Mount
docker run -d --mount type=bind,src=/host/path,dst=/app/config,readonly app

# Tmpfs Mount
docker run -d --mount type=tmpfs,dst=/tmp/keys,tmpfs-size=64m app

# Cleanup Dangling Volumes
docker volume prune -f
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [05 - Docker Networking](../05-Docker-Networking/README.md) |
