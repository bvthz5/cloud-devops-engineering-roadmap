# 05 - Non-Root Users and Least Privilege Execution

## 1. The Peril of Root Containers

By default, containers execute as `root` (UID 0). If a containerized application is compromised via Remote Code Execution (RCE) and escapes through a kernel vulnerability, the attacker possesses **root privileges on the host OS**!

---

## 2. Implementing Non-Root Execution

```dockerfile
FROM python:3.11-slim

# Create dedicated unprivileged system group and user
RUN groupadd -g 10001 appgroup && \
    useradd -u 10001 -g appgroup -s /sbin/nologin -d /app appuser

WORKDIR /app
COPY --chown=appuser:appgroup . /app

# Switch to non-root user
USER 10001:10001

EXPOSE 8080
ENTRYPOINT ["python", "main.py"]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Distroless Alpine and Scratch Base Images](./04-Distroless-Alpine-and-Scratch-Base-Images.md) | [Index](../../../README.md) | [06 - BuildKit Advanced Features and Cache Mounts →](./06-BuildKit-Advanced-Features-and-Cache-Mounts.md) |
