# 05 - Compose Profiles and Multi-Environment Overrides

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to managing multiple deployment environments (Development, Staging, Production), file inheritance with overrides, service activation via Profiles, and DRY configuration using YAML anchors.

---

## 🏛️ 1. What Are Multi-Environment Configurations?

In enterprise software engineering, applications run across multiple distinct deployment tiers:
- **Local Development:** Requires live source-code bind mounts, debuggers, exposed administrative ports, and mock services.
- **Testing & Continuous Integration (CI):** Requires ephemeral database containers, test runners, and automated seeders that exit upon completion.
- **Production:** Requires hardened immutable images, strict resource limits, non-root users, isolated private networks, and persistent cloud storage.

Attempting to manage these radically different requirements within a single monolithic configuration file results in configuration bloat and accidental deployment of debugging tools to production servers.

Docker Compose solves this challenge through two primary architectural patterns:
1. **Multi-File Overrides:** Layering specialized environment files over a common base configuration.
2. **Compose Profiles:** Selectively enabling or disabling specific services at runtime using tags.

---

## ⚙️ 2. The Multi-File Override Pattern

Docker Compose natively supports loading multiple configuration files simultaneously using the `-f` (file) flag. When multiple files are specified, Compose merges them in the order they are provided, with later files overriding or augmenting earlier declarations.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   COMPOSE MULTI-FILE MERGE STRATEGY                    │
└────────────────────────────────────────────────────────────────────────┘

  [ compose.yaml ]                     [ compose.override.yaml ]
  (Base declarations common             (Local development additions)
   to all environments)
           │                                      │
           └──────────────────┬───────────────────┘
                              ▼
           Merged Final Runtime Configuration
           - Preserves base images and networks
           - Adds local bind mounts and published debug ports
```

### The Default Override Rule:
If you run `docker compose up` without specifying any file flags, Compose automatically looks for and merges two files by default:
1. `compose.yaml` (or `docker-compose.yml`)
2. `compose.override.yaml` (or `docker-compose.override.yml`)

This allows developers to keep production-safe defaults in `compose.yaml` while checking an uncommitted `compose.override.yaml` into their local working tree for custom development settings.

### Production Environment Merging:
In continuous delivery pipelines, engineers explicitly specify the file hierarchy using the `-f` flag:
```bash
# Merges the base configuration with production-specific overrides
docker compose -f compose.yaml -f compose.prod.yaml up -d
```

---

## 🔬 3. Practical Example: Base vs. Development vs. Production

### 1. Base Configuration (`compose.yaml`):
Defines the core services, shared networks, and common environment settings:
```yaml
services:
  web:
    image: mycompany/ecommerce-web:v1.0.0
    restart: unless-stopped
    networks:
      - app_net
    environment:
      APP_ENV: production
      LOG_LEVEL: info

networks:
  app_net:
    driver: bridge
```

### 2. Local Development Override (`compose.override.yaml`):
Mounts live source code for hot-reloading and exposes debug ports to the host:
```yaml
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"          # Expose web server directly on developer laptop
      - "9229:9229"          # Node.js V8 debugger inspection port
    volumes:
      - ./:/app:delegated    # Live source code bind mount
    environment:
      APP_ENV: development
      LOG_LEVEL: debug
```

### 3. Production Override (`compose.prod.yaml`):
Enforces strict resource limits and binds ports exclusively to an internal reverse proxy:
```yaml
services:
  web:
    deploy:
      resources:
        limits:
          cpus: '2.0'
          memory: 1024M
        reservations:
          memory: 512M
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "5"
```

---

## 🏷️ 4. Docker Compose Profiles: Selective Service Activation

While multi-file overrides handle differences across deployment environments, **Compose Profiles** allow you to selectively activate auxiliary services within the same environment without creating separate files.

Services assigned one or more profiles are ignored by default when running `docker compose up`. They are only initialized if their specific profile is requested explicitly.

### Real-World Use Cases for Profiles:
- **Administrative Tools:** Database web viewers (e.g., pgAdmin, phpMyAdmin) that should only run when an engineer is debugging.
- **Monitoring & Observability:** Prometheus and Grafana stacks running alongside local services.
- **Automated Test Runners:** Integration testing containers that run on-demand in CI pipelines.

### Profile Implementation Example:
```yaml
services:
  api:
    image: mycompany/api:latest
    ports:
      - "8080:8080"
    # No profile assigned -> Runs by default on 'docker compose up'

  database:
    image: postgres:16-alpine
    # No profile assigned -> Runs by default on 'docker compose up'

  pgadmin:
    image: dpage/pgadmin4:latest
    ports:
      - "5050:80"
    profiles:
      - tools          # Only starts when 'tools' or 'debug' profile is active
      - debug

  integration-tests:
    build:
      context: ./tests
    profiles:
      - test           # Only starts when 'test' profile is active
```

### Executing Profile Commands:
```bash
# 1. Starts only 'api' and 'database' (Default services)
docker compose up -d

# 2. Starts default services PLUS the pgadmin container
docker compose --profile tools up -d

# 3. Runs only the integration test suite
docker compose --profile test run integration-tests
```

---

## 🧩 5. DRY Configuration with YAML Anchors and Aliases

To maintain clean, readable code and adhere to the "Don't Repeat Yourself" (DRY) principle, Compose supports native YAML 1.2 **Anchors** (`&`), **Aliases** (`*`), and **Merge Keys** (`<<`). This allows you to define reusable configuration fragments once and inject them across dozens of microservices.

```yaml
# 1. Define reusable configuration anchor using 'x-' extension field
x-common-config: &common-service-config
  restart: unless-stopped
  networks:
    - backend_net
  logging:
    driver: "json-file"
    options:
      max-size: "20m"
      max-file: "3"

services:
  auth-service:
    <<: *common-service-config      # Injects common settings!
    image: mycompany/auth:v1.0
    ports:
      - "8001:8001"

  billing-service:
    <<: *common-service-config      # Injects common settings!
    image: mycompany/billing:v1.0
    ports:
      - "8002:8002"

networks:
  backend_net:
    driver: bridge
```

---

## 📋 6. Summary and Navigation

Combining multi-file overrides, runtime profiles, and YAML anchors empowers DevOps teams to maintain a clean, maintainable configuration repository. This architecture guarantees that development, continuous integration, and production environments share common foundations while remaining strictly tailored to their operational requirements.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Environment Variables and Secrets Management](./04-Environment-Variables-and-Secrets-Management.md) | [Index](../../../README.md) | [06 - Production Deployments and Resource Limits →](./06-Production-Deployments-and-Resource-Limits.md) |
