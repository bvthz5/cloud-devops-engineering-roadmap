# 03 - Essential Docker CLI Commands

## 1. Operational Command Matrix

```bash
# 1. Run container in background with restart policy and port publishing
docker run -d \
  --name webapp \
  --restart unless-stopped \
  -p 8080:80 \
  -e ENV=production \
  -v app_data:/var/data \
  nginx:alpine

# 2. Inspect real-time container resource utilization (CPU, RAM, Net I/O)
docker stats --no-stream

# 3. Stream logs with timestamps and limit to last 100 lines
docker logs -f --tail 100 -t webapp

# 4. Execute an interactive shell inside a running container
docker exec -it webapp /bin/sh

# 5. Extract structured metadata via Go template
docker inspect --format '{{ .State.Health.Status }}' webapp

# 6. Deep system cleanup: removes unused containers, dangling images, build cache
docker system prune -a --volumes -f
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Container Lifecycle & States](./02-Container-Lifecycle-and-Process-States.md) | [README](./README.md) | [04 - Resource Constraints & Limits](./04-Resource-Constraints-and-Limits.md) |
