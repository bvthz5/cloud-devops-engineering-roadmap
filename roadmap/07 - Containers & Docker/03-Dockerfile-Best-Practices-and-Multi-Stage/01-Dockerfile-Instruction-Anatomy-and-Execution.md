# 01 - Dockerfile Instruction Anatomy and Execution

## 1. Key Instructions Explained

```dockerfile
# 1. ARG vs ENV
ARG APP_VERSION=1.0.0    # Available ONLY during build time! Not persisted in final image.
ENV APP_PORT=8080        # Persisted into final container runtime environment.

# 2. WORKDIR
WORKDIR /app             # Creates directory if it does not exist; sets execution directory.

# 3. COPY vs ADD
COPY src/ /app/src/      # Transparently copies local files (Recommended).
ADD archive.tar.gz /app/ # Automatically unpacks tar archives; downloads remote URLs (Use cautiously!).

# 4. ENTRYPOINT vs CMD
ENTRYPOINT ["node", "server.js"] # The executable binary (Exec form JSON array mandatory!)
CMD ["--port", "8080"]           # Default arguments appended to ENTRYPOINT (easily overridden by CLI)
```

---

## 2. Shell Form vs Exec Form Pitfall

```dockerfile
# BAD: Shell Form (Wraps in /bin/sh -c)
CMD node server.js
# Result: /bin/sh is PID 1! When 'docker stop' sends SIGTERM, /bin/sh IGNORES IT,
# preventing graceful shutdown and corrupting state!

# GOOD: Exec Form (Direct Execution)
CMD ["node", "server.js"]
# Result: node is PID 1 and receives SIGTERM directly.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (02-Docker-Architecture-and-CLI)](../02-Docker-Architecture-and-CLI/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Layer Caching Optimization and Build Order →](./02-Layer-Caching-Optimization-and-Build-Order.md) |
