# 10 - Hands-On Practice Labs

## Lab 1: Resilient Web + Redis Stack with Healthchecks

### Objective
Create a `compose.yaml` with a Node.js web app and Redis cache, enforcing healthchecks and resource limits.

### Implementation (`compose.yaml`)
```yaml
services:
  web:
    image: node:20-alpine
    command: ["node", "-e", "console.log('App running'); setInterval(()=>{}, 1000)"]
    ports:
      - "3000:3000"
    depends_on:
      redis:
        condition: service_healthy

  redis:
    image: redis:alpine
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 3s
      retries: 3
    deploy:
      resources:
        limits:
          memory: 256M
```

```bash
# Start and inspect
docker compose up -d
docker compose ps
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
