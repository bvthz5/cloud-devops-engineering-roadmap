# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the architectural difference between `depends_on` with and without `condition: service_healthy`?
**Answer**: Basic `depends_on` only enforces container process startup order; Docker launches the dependent container the millisecond the parent container process starts. `condition: service_healthy` halts the dependent container until the parent container's defined `healthcheck` command succeeds with exit code 0, eliminating database startup race conditions.

### Q2: How does Docker Compose manage service discovery across containers?
**Answer**: Docker Compose automatically creates a default user-defined bridge network for the project. Each service is assigned a DNS record matching its service name in the Compose YAML, resolved via Docker's embedded DNS server (`127.0.0.11`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
