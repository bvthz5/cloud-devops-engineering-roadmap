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
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (07-Container-Security-and-Image-Scanning) →](../07-Container-Security-and-Image-Scanning/01-Container-Threat-Modeling-and-Escape-Vectors.md) |
