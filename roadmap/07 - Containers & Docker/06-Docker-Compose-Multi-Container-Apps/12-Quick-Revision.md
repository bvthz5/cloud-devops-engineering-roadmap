# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Essential Compose CLI Commands
docker compose up -d               # Start services in background
docker compose down -v             # Stop and remove containers + volumes
docker compose logs -f <service>   # Stream service logs
docker compose ps                  # List container states
docker compose config              # View merged configuration
```

```yaml
# Healthcheck Golden Template
healthcheck:
  test: ["CMD-SHELL", "curl -f http://localhost:8080/health || exit 1"]
  interval: 10s
  timeout: 5s
  retries: 3
  start_period: 15s
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [07 - Container Security & Scanning](../07-Container-Security-and-Image-Scanning/README.md) |
