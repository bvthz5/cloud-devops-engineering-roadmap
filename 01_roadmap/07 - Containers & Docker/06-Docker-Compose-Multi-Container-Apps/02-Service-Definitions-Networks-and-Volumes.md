# 02 - Service Definitions, Networks, and Volumes in Docker Compose

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to defining multi-container application services, configuring isolated software-defined networks, and establishing persistent volume storage in Docker Compose.

---

## 🏛️ 1. What Is Docker Compose & Why Are Service Definitions Needed?

In modern microservice architectures, a production application rarely runs as an isolated single container. A typical modern web application requires a frontend reverse proxy, an application programming interface (API) backend, a relational database, an in-memory caching layer, and background asynchronous worker queues.

Managing these containers individually using standalone terminal commands like `docker run` quickly becomes unmanageable, error-prone, and fragile. Engineers must manually track arbitrary port forwardings, remember exact environment variables, and create custom virtual bridge networks by hand.

**Docker Compose** solves this operational complexity by providing a declarative YAML specification file (`docker-compose.yml` or `compose.yaml`). Through this file, you declare the desired end-state of your entire multi-container architecture in human-readable code. When you execute `docker compose up -d`, the Compose engine automatically reads this declaration, provisions all necessary virtual network switches, allocates persistent storage volumes, and starts all containers in their mathematically correct dependency order.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   DOCKER COMPOSE TOP-LEVEL ARCHITECTURE                │
│                                                                        │
│  services:                                                             │
│    Declares each independent container workload (web, api, db, cache). │
│                                                                        │
│  networks:                                                             │
│    Declares isolated virtual bridge switches connecting services.     │
│                                                                        │
│  volumes:                                                              │
│    Declares persistent storage drives outside the container lifecycle. │
│                                                                        │
│  secrets:                                                              │
│    Declares sensitive credentials mounted into /run/secrets/ safely.   │
│                                                                        │
│  configs:                                                              │
│    Declares non-sensitive configuration files (nginx.conf, etc.).      │
└────────────────────────────────────────────────────────────────────────┘
```

---

## ⚙️ 2. Comprehensive Service Definitions (`services:`)

The **`services`** top-level block defines the actual container workloads that compose your application. Each named key under `services:` represents an independent container definition with its own image, ports, storage, environment, and networking rules.

### 2.1 Image vs. Build: How Containers Are Sourced
A service must define either an existing pre-built container image to pull from a registry or instructions on how to build one from a local Dockerfile:

1. **`image` Directive:** Specifies an image name and version tag from a container registry such as Docker Hub, Amazon ECR, or GitHub Packages.
   ```yaml
   services:
     database:
       image: postgres:16-alpine
   ```
2. **`build` Directive:** Instructs Docker Compose to build an image dynamically from source code. You specify the directory path containing the build context, the path to the Dockerfile, and optional build arguments:
   ```yaml
   services:
     api:
       build:
         context: ./backend             # Directory sent to Docker daemon as build context
         dockerfile: Dockerfile.prod    # Custom Dockerfile name
         args:
           APP_VERSION: "2.4.0"         # Build-time arguments (ARG)
         target: production             # Target stage in a multi-stage build
   ```

---

### 2.2 Port Forwarding (`ports:`) vs. Port Exposing (`expose:`)
Understanding the difference between publishing ports and exposing ports is critical for securing multi-tier architectures:

- **`ports:` (Host-to-Container Publishing):**
  Maps a port on the physical host machine into a port inside the container using the syntax `"HOST_PORT:CONTAINER_PORT"`. This makes the service accessible from the outside internet and your local web browser.
  ```yaml
  ports:
    - "8080:80"       # Anyone accessing http://localhost:8080 reaches container port 80
    - "127.0.0.1:5432:5432" # Secure binding: only accessible from local host, not public internet!
  ```
- **`expose:` (Internal Container Documentation):**
  Declares that a container listens on a specific port **without** exposing that port to the host machine or public internet. Sibling containers attached to the same Compose network can still connect to this port, but outside attackers cannot reach it.
  ```yaml
  expose:
    - "6379"          # Redis port visible to internal services only; completely hidden from host!
  ```

---

### 2.3 Environment Configuration: `environment` vs. `env_file`
Environment variables allow containers to remain completely decoupled from environment-specific configurations like database URLs, API keys, and debug flags:

- **`environment:` Directive:** Defines literal key-value pairs directly in the Compose file.
  ```yaml
  environment:
    - NODE_ENV=production
    - PORT=3000
    - LOG_LEVEL=info
  ```
- **`env_file:` Directive:** Reads variables from an external plain-text file (typically `.env`). This prevents hardcoding sensitive secrets into version-controlled Git repositories.
  ```yaml
  env_file:
    - .env.production
  ```

---

### 2.4 Container Restart Policies (`restart:`)
The `restart` directive dictates how Docker handles containers when their processes exit or when the host server reboots:

1. **`no` (Default):** Do not restart the container automatically under any circumstances.
2. **`always`:** Always restart the container regardless of exit code. If manually stopped, it restarts on server reboot.
3. **`on-failure`:** Restart only if the container process crashes with a non-zero exit code (e.g. exit code 1 or 137). You can specify retry limits: `on-failure:5`.
4. **`unless-stopped` (Industry Standard):** Always restart the container if it crashes or if the host reboots, **unless** the system administrator explicitly stopped it using `docker compose stop`.

---

## 🌐 3. Multi-Tier Networking in Docker Compose (`networks:`)

### 3.1 The Default Network Behavior
When you run `docker compose up`, Compose automatically creates a single user-defined bridge network named `<project_name>_default`. Every service that does not explicitly specify a network joins this default network automatically. 

Within this network, every container can reach every sibling container by its service name using Docker's internal DNS server (`127.0.0.11`). For example, your backend code can connect directly to `postgres://user:pass@db:5432/mydb`, and Docker automatically resolves `db` to the current private IP of the database container.

### 3.2 Enterprise Multi-Tier Network Isolation
In secure enterprise deployments, leaving all services on a single shared default network is an architectural vulnerability. If the public-facing web server is compromised, an attacker can directly probe the internal database.

By declaring dedicated **Frontend** and **Backend** networks, you establish strict security perimeters:
- The **Web Reverse Proxy** lives only on `frontend_net`.
- The **API Backend** sits on both `frontend_net` and `backend_net` (acting as a secure bridge).
- The **Database and Redis** live strictly on `backend_net`.
- *Result:* The database is physically and logically unreachable from the public reverse proxy!

```text
                  PUBLIC INTERNET
                         │
                         ▼ (Port 80 / 443)
              ┌─────────────────────┐
              │   web (Nginx)       │
              └──────────┬──────────┘
                         │
        ═════════════════╪═════════════════  frontend_net
                         │
              ┌──────────┴──────────┐
              │   api (Node/Python) │
              └──────────┬──────────┘
                         │
        ═════════════════╪═════════════════  backend_net (ISOLATED)
                         │
              ┌──────────┴──────────┐
              │   db (PostgreSQL)   │
              └─────────────────────┘
         (Completely hidden from web!)
```

---

## 🗄️ 4. Persistent Storage & Volumes in Compose (`volumes:`)

### 4.1 Why Containers Require External Volumes
Containers are fundamentally **ephemeral**. The writable layer of a container exists only as long as that specific container instance is running. When you run `docker compose down`, the container is deleted, and any data written inside its virtual disk is permanently lost.

To maintain state across deployments, upgrades, and host reboots, you must store data in external storage mounts.

### 4.2 The 3 Types of Volume Mounts:

1. **Named Volumes (Managed by Docker):**
   Declared under the top-level `volumes:` section and managed entirely by Docker inside `/var/lib/docker/volumes/`. This is the **preferred standard for production databases** because Docker manages permissions, performance, and portability.
   ```yaml
   services:
     db:
       image: postgres:16-alpine
       volumes:
         - pg_data:/var/lib/postgresql/data  # Format: VOLUME_NAME:CONTAINER_PATH

   volumes:
     pg_data:                                # Declares the named volume
   ```
2. **Bind Mounts (Host Directory Mapping):**
   Maps an exact directory on your host computer into a directory inside the container. This is ideal for **local software development** because editing code on your laptop reflects instantly inside the running container without rebuilding images:
   ```yaml
   services:
     frontend:
       image: node:20-alpine
       volumes:
         - ./src:/app/src:ro                 # Maps local ./src folder, mounted Read-Only (:ro)
   ```
3. **tmpfs Mounts (High-Speed RAM Storage):**
   Stores files directly in the host machine's random-access memory (RAM). Data is never written to disk and disappears the moment the container stops. Ideal for temporary tokens, session keys, and cryptographic certificates:
   ```yaml
   services:
     worker:
       image: alpine
       tmpfs:
         - /tmp/secure_tokens
   ```

---

## 🔒 5. Managing Secrets & Credentials (`secrets:`)

Hardcoding production database passwords into `docker-compose.yml` or checking `.env` files into GitHub creates severe security compliance violations. Docker Compose provides a native **`secrets:`** mechanism:

1. The secret file resides locally on disk (e.g. `./secrets/db_password.txt`) with strict `chmod 600` file permissions.
2. Compose reads the secret and mounts it as an in-memory file at `/run/secrets/<secret_name>` inside the designated container.
3. The password is never visible in `docker inspect`, never exposed in environment variables, and never committed to Git!

---

## 🚀 6. Complete Production Multi-Tier `docker-compose.yml`

Here is a complete, production-grade, multi-tier application template demonstrating all of the concepts covered above:

```yaml
version: "3.8"

# ==============================================================================
# 1. SERVICES DECLARATION (Container Workloads)
# ==============================================================================
services:

  # ── Tier 1: Public Reverse Proxy (Nginx) ────────────────────────────────────
  web:
    image: nginx:alpine
    container_name: production_web_proxy
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/certs:/etc/ssl/certs:ro
    networks:
      - frontend_net
    depends_on:
      api:
        condition: service_healthy

  # ── Tier 2: Application API Backend (Node.js) ───────────────────────────────
  api:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: production_api_server
    restart: unless-stopped
    environment:
      - NODE_ENV=production
      - DB_HOST=db
      - DB_PORT=5432
      - DB_NAME=ecommerce_db
      - REDIS_URL=redis://cache:6379
    secrets:
      - db_password
    networks:
      - frontend_net
      - backend_net
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://localhost:3000/health || exit 1"]
      interval: 10s
      timeout: 3s
      retries: 3
      start_period: 15s

  # ── Tier 3: Production Database (PostgreSQL) ────────────────────────────────
  db:
    image: postgres:16-alpine
    container_name: production_postgres_db
    restart: always
    environment:
      POSTGRES_DB: ecommerce_db
      POSTGRES_USER: app_admin
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    secrets:
      - db_password
    volumes:
      - pg_data:/var/lib/postgresql/data
      - ./database/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    networks:
      - backend_net
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app_admin -d ecommerce_db"]
      interval: 5s
      timeout: 3s
      retries: 5
      start_period: 20s

  # ── Tier 4: In-Memory Cache (Redis) ─────────────────────────────────────────
  cache:
    image: redis:7-alpine
    container_name: production_redis_cache
    restart: always
    command: ["redis-server", "--appendonly", "yes", "--maxmemory", "512mb", "--maxmemory-policy", "allkeys-lru"]
    volumes:
      - redis_data:/data
    networks:
      - backend_net

# ==============================================================================
# 2. NETWORKS DECLARATION (Software-Defined Isolated Bridges)
# ==============================================================================
networks:
  frontend_net:
    driver: bridge
  backend_net:
    driver: bridge
    internal: true   # Blocks all outbound internet traffic from backend servers!

# ==============================================================================
# 3. VOLUMES DECLARATION (Persistent Storage Allocations)
# ==============================================================================
volumes:
  pg_data:
    driver: local
  redis_data:
    driver: local

# ==============================================================================
# 4. SECRETS DECLARATION (Encrypted File-Based Credentials)
# ==============================================================================
secrets:
  db_password:
    file: ./secrets/db_password.txt
```

---

## 💻 7. Operational Command Cheat Sheet

```bash
# 1. Build images and start all services in detached background mode
docker compose up -d --build

# 2. Inspect active services, health status, and published ports
docker compose ps

# 3. Follow live output logs from all services or a specific service
docker compose logs -f api

# 4. Execute an interactive shell inside a running service container
docker compose exec api sh

# 5. Connect to the database shell using service DNS
docker compose exec db psql -U app_admin -d ecommerce_db

# 6. Scale backend API horizontally to handle surging traffic
docker compose up -d --scale api=3

# 7. Stop and cleanly remove all containers and virtual networks (keeps volumes safe!)
docker compose down

# 8. Stop and completely destroy containers, networks, AND persistent storage volumes
docker compose down -v
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Docker Compose v2 Specification and Architecture](./01-Docker-Compose-v2-Specification-and-Architecture.md) | [Index](../../../README.md) | [03 - Dependency Management and Healthchecks →](./03-Dependency-Management-and-Healthchecks.md) |
