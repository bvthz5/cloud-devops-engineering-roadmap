# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Boot-Loop Database Migration Failure

### Context & Incident
A company automated deployments using `docker compose up -d`. On deployment, the API container crashed repeatedly into `CrashLoopBackOff`, preventing releases.

### Root Cause
The API container executed database schema migrations upon boot. Because `depends_on: [ db ]` only waited for the database container to spawn, the API attempted migrations before PostgreSQL finished initializing its database cluster files.

### Solution: Healthcheck Gating
```yaml
depends_on:
  db:
    condition: service_healthy
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Production Deployments and Resource Limits](./06-Production-Deployments-and-Resource-Limits.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
