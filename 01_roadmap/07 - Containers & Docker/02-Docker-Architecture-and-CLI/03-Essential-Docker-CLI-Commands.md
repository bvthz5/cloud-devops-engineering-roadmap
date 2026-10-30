# 03 - Essential Docker CLI Commands & Operations

## 1. Container Management Commands Reference (`docker container <subcommand>`)

```text
┌──────────────────────┬────────────────────────────────────────────────────────┐
│ Command Syntax       │ Worked Real-World Example                              │
├──────────────────────┼────────────────────────────────────────────────────────┤
│ 1. Create Container  │ docker container create --name web httpd               │
│ 2. Run Container     │ docker container run -d --name web -p 8080:80 httpd    │
│ 3. List Running      │ docker container ls                                    │
│    List All          │ docker container ls -a                                 │
│ 4. Start / Stop      │ docker container start web / docker container stop web │
│    Restart / Pause   │ docker container restart web / docker pause web        │
│ 5. Attach Terminal   │ docker container attach web (Hooks to PID 1)           │
│                      │ Detach escape sequence: Ctrl + P then Ctrl + Q         │
│ 6. Inspect & Stats   │ docker container inspect web / docker container stats  │
│ 7. Execute Command   │ docker container exec -it web bash                     │
│ 8. Copy Files        │ docker container cp index.html web:/usr/local/apache2/ │
│ 9. Remove Container  │ docker container rm -f web                             │
│ 10. Update Resources │ docker container update --memory 512m --cpus 1 web     │
└──────────────────────┴────────────────────────────────────────────────────────┘
```

---

## 2. Docker Flags & Short Keys Cheat Sheet

```text
┌─────────────────┬───────────┬────────────────────────────────────────────────────────┐
│ Command         │ Long Flag │ Short / Description                                    │
├─────────────────┼───────────┼────────────────────────────────────────────────────────┤
│ docker run      │ --detach  │ -d  (Run in background)                                │
│                 │ --tty     │ -t  (Allocate pseudo-TTY)                              │
│                 │ --interact│ -i  (Keep STDIN open)                                  │
│                 │ --publish │ -p HOST:CONTAINER (Port forward, e.g. -p 8080:80)      │
│                 │ --volume  │ -v HOST:CONTAINER (Volume mount)                       │
│                 │ --env     │ -e KEY=val (Set environment variable)                  │
│                 │ --memory  │ -m 512m (Limit RAM usage)                              │
│                 │ --rm      │ Automatically deletes container upon exit              │
├─────────────────┼───────────┼────────────────────────────────────────────────────────┤
│ docker exec     │ -it       │ Combined: Interactive terminal shell                   │
│                 │ -u        │ --user (Run command as specific UID or username)       │
│                 │ -w        │ --workdir (Working directory for the command)          │
├─────────────────┼───────────┼────────────────────────────────────────────────────────┤
│ docker ps       │ -a        │ --all (Show all containers including exited)           │
│                 │ -q        │ --quiet (Display numeric container IDs only)           │
│                 │ -f        │ --filter (e.g. -f "status=exited")                     │
├─────────────────┼───────────┼────────────────────────────────────────────────────────┤
│ docker build    │ -t        │ --tag (Name and tag image, e.g. -t myapp:1.0)          │
│                 │ -f        │ --file (Path to custom Dockerfile)                     │
│                 │ --no-cache│ Rebuild image without using layer cache                │
├─────────────────┼───────────┼────────────────────────────────────────────────────────┤
│ docker logs     │ -f        │ --follow (Stream output live)                          │
│                 │ -t        │ --timestamps (Display log timestamps)                  │
│                 │ --tail    │ Show last N lines (--tail 100)                         │
└─────────────────┴───────────┴────────────────────────────────────────────────────────┘
```

---

## 3. Operational Command Matrix

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
| [← 02 - Container Lifecycle and Process States](./02-Container-Lifecycle-and-Process-States.md) | [Index](../../../README.md) | [04 - Resource Constraints and Limits →](./04-Resource-Constraints-and-Limits.md) |
