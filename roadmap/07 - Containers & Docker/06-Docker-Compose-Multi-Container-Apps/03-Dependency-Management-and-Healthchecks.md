# 03 - Dependency Management and Healthchecks

## 1. The Startup Order Race Condition

In Compose v1, `depends_on: [ "db" ]` only waited for the database container to **start**, not for the database daemon to **accept network connections**. Application web servers would boot in 1 second, attempt to connect to Postgres while it was still initializing, crash with connection refused, and exit!

---

## 2. Solution: `service_healthy` Dependency

In Compose v2, configure an active healthcheck on the upstream dependency and gate the downstream service:

```yaml
services:
  app:
    image: myapp:v1
    depends_on:
      postgres:
        condition: service_healthy

  postgres:
    image: postgres:16-alpine
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres -d mydb"]
      interval: 5s
      timeout: 2s
      retries: 5
      start_period: 10s # Grace period during cold boots
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Service Definitions & Networks](./02-Service-Definitions-Networks-and-Volumes.md) | [README](./README.md) | [04 - Environment & Secrets Management](./04-Environment-Variables-and-Secrets-Management.md) |
