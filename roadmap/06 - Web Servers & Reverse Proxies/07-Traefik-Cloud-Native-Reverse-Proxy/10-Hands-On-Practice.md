# 10 - Hands-On Practice Labs

## Lab 1: Multi-Container Microservices with Traefik and Docker Compose

### Objective
Deploy Traefik alongside two microservices (`api` and `web`) with automatic path routing and basic authentication.

### Implementation
```yaml
version: "3.8"

services:
  traefik:
    image: traefik:v3.0
    command:
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080" # Dashboard
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"

  api-service:
    image: hashicorp/http-echo
    command: ["-text=API Microservice Response"]
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.api.entrypoints=web"
      - "traefik.http.routers.api.rule=Host(`localhost`) && PathPrefix(`/api`)"
      - "traefik.http.services.api.loadbalancer.server.port=5678"

  web-service:
    image: hashicorp/http-echo
    command: ["-text=Web Frontend Response"]
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.web.entrypoints=web"
      - "traefik.http.routers.web.rule=Host(`localhost`)"
      - "traefik.http.services.web.loadbalancer.server.port=5678"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
