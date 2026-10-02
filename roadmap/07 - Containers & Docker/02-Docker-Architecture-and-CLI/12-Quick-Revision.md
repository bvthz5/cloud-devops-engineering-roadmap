# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Docker Resource Limits
docker run -d --cpus="2" --memory="1g" --memory-swap="1g" --pids-limit=200 myapp

# Container Inspection
docker inspect --format '{{.State.Pid}}' my_container

# Clean all unused resources
docker system prune -a --volumes -f

# Daemon Stack:
# CLI -> dockerd -> containerd -> containerd-shim -> runc -> Application PID 1
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [03 - Dockerfile Best Practices & Multi-Stage](../03-Dockerfile-Best-Practices-and-Multi-Stage/README.md) |
