# 03 - Docker and Docker Compose Integration

## 1. Dynamic Discovery via Container Labels

When running with the Docker provider, developers declare reverse proxy routing directly within `docker-compose.yml` using container labels. When the container starts, Traefik immediately reads the labels and configures routing without touching any files!

```yaml
version: "3.8"

services:
  traefik:
    image: traefik:v3.0
    command:
      - "--api.dashboard=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"

  whoami:
    image: traefik/whoami
    labels:
      # 1. Enable Traefik for this container
      - "traefik.enable=true"

      # 2. Router: Listen on 'web' entrypoint for app.localhost
      - "traefik.http.routers.whoami.entrypoints=web"
      - "traefik.http.routers.whoami.rule=Host(`app.localhost`)"

      # 3. Middleware: Strip /api prefix
      - "traefik.http.middlewares.strip-api.stripprefix.prefixes=/api"
      - "traefik.http.routers.whoami.middlewares=strip-api"

      # 4. Target container port
      - "traefik.http.services.whoami.loadbalancer.server.port=80"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Core Concepts](./02-Core-Concepts-EntryPoints-Routers-Middlewares-Services.md) | [README](./README.md) | [04 - Kubernetes Ingress & Gateway API](./04-Kubernetes-Ingress-and-Gateway-API-with-Traefik.md) |
