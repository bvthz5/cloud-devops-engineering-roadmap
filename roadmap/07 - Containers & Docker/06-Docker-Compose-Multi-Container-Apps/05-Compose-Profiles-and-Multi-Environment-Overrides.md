# 05 - Compose Profiles and Multi-Environment Overrides

## 1. Multi-Environment File Overrides

Docker Compose automatically merges `compose.override.yaml` with `compose.yaml`:

```bash
# Production deployment with explicit production file
docker compose -f compose.yaml -f compose.prod.yaml up -d
```

---

## 2. Compose Profiles

Profiles allow optional services (e.g. database debug GUIs, testing runners, metrics scrapers) to remain dormant unless explicitly invoked:

```yaml
services:
  app:
    image: myapp

  pgadmin:
    image: dpage/pgadmin4
    profiles:
      - debug # Only started when --profile debug is supplied!
```

```bash
# Start standard app
docker compose up -d

# Start app AND debug tools
docker compose --profile debug up -d
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Environment & Secrets Management](./04-Environment-Variables-and-Secrets-Management.md) | [README](./README.md) | [06 - Production Deployments & Limits](./06-Production-Deployments-and-Resource-Limits.md) |
