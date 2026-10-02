# 06 - Docker Compose Multi-Container Apps

Docker Compose is the standard tool for defining and running multi-container Docker applications. Through declarative YAML files, Compose coordinates container lifecycles, service dependencies, network topographies, healthchecks, and persistent volumes across development, testing, and production environments.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Compose v2 Specification & Architecture](./01-Docker-Compose-v2-Specification-and-Architecture.md) | Compose v2 CLI plugin (`docker compose`), Compose Spec, migration from v1. |
| 02 | [Service Definitions, Networks & Volumes](./02-Service-Definitions-Networks-and-Volumes.md) | Defining multi-tier topologies, custom networks, persistent volume declarations. |
| 03 | [Dependency Management & Healthchecks](./03-Dependency-Management-and-Healthchecks.md) | `depends_on` with `condition: service_healthy`, defining interval/timeout probes. |
| 04 | [Environment Variables & Secrets Management](./04-Environment-Variables-and-Secrets-Management.md) | `.env` file precedence, variable interpolation, Compose file secrets. |
| 05 | [Compose Profiles & Multi-Environment Overrides](./05-Compose-Profiles-and-Multi-Environment-Overrides.md) | `docker-compose.override.yml`, targeting staging/prod, debugging with profiles. |
| 06 | [Production Deployments & Resource Limits](./06-Production-Deployments-and-Resource-Limits.md) | `deploy.resources.limits`, memory, CPU pinning, logging drivers, restart policies. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Database startup race condition crash, plain `.env` Git leak. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Validating merged configs (`docker compose config`), viewing logs, restart debugging. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior DevOps/SRE questions on Compose v2, healthcheck orchestration, and scaling. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Resilient 3-tier App with Healthchecks; Lab 2: Compose file overrides; Lab 3: Secrets. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for compose.yaml syntax, CLI commands, and healthcheck flags. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Docker Networking](../05-Docker-Networking/README.md) | [README](./README.md) | [01 - Compose v2 Specification](./01-Docker-Compose-v2-Specification-and-Architecture.md) |
