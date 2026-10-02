# 06 - IaC Platform Engineering

## Self-Service Infrastructure

```text
Developer Request: "I need a PostgreSQL database"
    |
    v
Internal Portal / Service Catalog
    |
    v
Triggers Terraform module with pre-approved parameters
    |
    v
Terraform Cloud creates RDS with security, monitoring, backups
    |
    v
Developer gets connection string (automated)
```

Tools for self-service IaC: Backstage (Spotify), Port, Humanitec, Terraform Cloud with variable sets.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Performance Optimization](./05-Performance-Optimization-Large-States.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
