# 01 - Docker Compose v2 Specification and Architecture

## 1. Docker Compose v1 (Python) vs v2 (Go Plugin)

- **Compose v1 (`docker-compose`)**: Legacy standalone Python binary. Slower startup, separate release cycle, deprecated.
- **Compose v2 (`docker compose`)**: Written in Go as a native Docker CLI plugin (`docker compose`). Offers full parity with the modern **Compose Specification** and high-speed execution.

---

## 2. Default Filename Conventions
Compose automatically detects configuration files in the following order:
1. `compose.yaml` (Recommended modern standard)
2. `compose.yml`
3. `docker-compose.yaml`
4. `docker-compose.yml`

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Service Definitions & Networks](./02-Service-Definitions-Networks-and-Volumes.md) |
