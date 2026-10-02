# 01 - Traefik Architecture and Dynamic Discovery

## 1. Static Configuration vs Dynamic Configuration

Traefik strictly separates configuration into two independent layers:

```text
1. Static Configuration (Startup Configuration):
   - Defined in traefik.yml, CLI flags, or environment variables.
   - Defines infrastructure fundamentals: EntryPoints (ports 80/443), Providers, Log levels, CertificateResolvers.
   - Requires process restart to change.

2. Dynamic Configuration (Runtime Configuration):
   - Discovered automatically from Providers (Docker daemon, Kubernetes API, Consul, Redis).
   - Defines active routes, middlewares, services, TLS certificates, and canary splits.
   - Continuously updated in memory with ZERO restarts and ZERO dropped connections!
```

```text
[ Container / Pod Spawns with Labels ]
                 │
                 ▼
[ Provider (Docker / Kube API) ] ──(Event Stream)──► [ Traefik Engine ] ──► [ Instant Route Sync ]
```

---

## 2. Minimal Static Configuration (`traefik.yml`)

```yaml
# Entrypoints: Network listeners
entryPoints:
  web:
    address: ":80"
  websecure:
    address: ":443"

# Providers: Where Traefik discovers routing configuration
providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
    exposedByDefault: false # Security Best Practice: Only expose explicitly tagged containers
  file:
    directory: "/etc/traefik/dynamic"
    watch: true

# API and Web Dashboard
api:
  dashboard: true
  insecure: false

log:
  level: INFO
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Core Concepts](./02-Core-Concepts-EntryPoints-Routers-Middlewares-Services.md) |
