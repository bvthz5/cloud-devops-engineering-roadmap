# 06 - Production Deployments and Resource Limits

## 1. Enforcing Hardware Limits in Compose

```yaml
services:
  api:
    image: mycompany/api:v1.2
    restart: always
    deploy:
      resources:
        limits:
          cpus: '2.0'
          memory: 2G
        reservations:
          cpus: '0.5'
          memory: 512M
    logging:
      driver: "json-file"
      options:
        max-size: "20m"
        max-file: "3"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Compose Profiles and Multi Environment Overrides](./05-Compose-Profiles-and-Multi-Environment-Overrides.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
