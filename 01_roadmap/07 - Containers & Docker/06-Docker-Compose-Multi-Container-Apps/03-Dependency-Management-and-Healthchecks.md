# 03 - Dependency Management and Healthchecks in Docker Compose

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to container startup ordering, service dependencies, healthcheck probes, database readiness synchronization, and resilient container restarts in Docker Compose.

---

## 🏛️ 1. What Is Dependency Management in Multi-Container Apps?

In a multi-tier microservice architecture, applications do not operate in a vacuum. A backend API server depends on a relational database like PostgreSQL to be running and accepting connections. A caching gateway depends on Redis. A message worker depends on RabbitMQ or Apache Kafka.

If all containers are launched simultaneously without orchestration, dependent services will attempt to connect to downstream infrastructure before it is initialized. This results in connection refused errors, crash loops, and unstable application bootstrapping.

**Dependency management** is the architectural practice of defining explicit startup and shutdown order relationships between services, ensuring that dependent systems are only contacted once prerequisite services are fully operational.

---

## ⚙️ 2. The Difference Between "Running" and "Ready"

One of the most common mistakes made by engineers new to Docker is assuming that because a container status is reported as **Running**, the application inside it is ready to receive network traffic.

```text
┌────────────────────────────────────────────────────────────────────────┐
│             CONTAINER STATUS VS. APPLICATION READINESS                 │
└────────────────────────────────────────────────────────────────────────┘

 1. Container Initialized (Process Starts)
    [ PID 1 (postgres) launched ] ──> Docker Engine marks container as: "RUNNING"
                                      ⚠️ Database engine is NOT ready yet!
                                      ⚠️ Reading configuration files...
                                      ⚠️ Replaying transaction logs...
                                      ⚠️ Initializing shared memory buffers...

 2. Internal Readiness Probe Succeeds
    [ pg_isready -U user ] ───────> Returns Exit Code 0!
                                      ✅ Database is listening on port 5432!
                                      ✅ Docker Engine marks status as: "HEALTHY"
```

A container enters the `running` state the millisecond the Linux kernel spawns PID 1 inside the container namespace. However, complex enterprise software like PostgreSQL, MySQL, or Elasticsearch often requires 5 to 30 seconds after process startup to initialize internal memory structures, run data migrations, replay write-ahead logs, and bind to network sockets.

If a backend service connects during this window, it will encounter a connection rejection. To solve this, Docker Compose provides **Healthcheck Probes** coupled with **Condition-Aware Dependency Rules**.

---

## 🔬 3. The `depends_on` Directive and Condition Probes

In Compose v1, the `depends_on` directive only supported a simple list of service names, ensuring that dependent containers were started in sequence. However, Compose did not wait for the service to be ready before starting the next container.

In Compose v2, `depends_on` supports explicit **conditions**, enabling deterministic synchronization based on real-time operational health.

### The Three Dependency Conditions:
1. **`service_started` (Default):** Compose starts the dependent container as soon as the prerequisite container process is spawned. (Equivalent to legacy v1 behavior).
2. **`service_healthy`:** Compose waits until the prerequisite container's internal healthcheck probe returns a zero exit code and enters the `healthy` status before launching the dependent service.
3. **`service_completed_successfully`:** Compose waits until the prerequisite container finishes its task with an exit code of 0 before proceeding. This is ideal for one-off migration scripts, seeders, or database schema setup jobs.

---

## 📝 4. Complete Production-Grade Implementation

The following complete `compose.yaml` demonstrates how to orchestrate a database, a database migration job, and a web API using condition-aware dependencies and healthcheck probes:

```yaml
services:
  # 1. Relational Database with Native Healthcheck
  postgres-db:
    image: postgres:16-alpine
    container_name: production_postgres
    environment:
      POSTGRES_DB: app_production
      POSTGRES_USER: postgres_admin
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    secrets:
      - db_password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - backend_network
    healthcheck:
      # Use pg_isready to query the PostgreSQL daemon internally
      test: ["CMD-SHELL", "pg_isready -U postgres_admin -d app_production"]
      interval: 5s          # Run healthcheck probe every 5 seconds
      timeout: 5s           # Fail probe if response takes longer than 5 seconds
      retries: 5            # Mark unhealthy after 5 consecutive failures
      start_period: 10s     # Grace period during startup before failures count

  # 2. Database Migration Seeder (Runs once and exits cleanly)
  db-migration:
    image: mycompany/api-migrations:v1.2.0
    container_name: production_migrations
    environment:
      DATABASE_URL: postgresql://postgres_admin@postgres-db:5432/app_production
    depends_on:
      postgres-db:
        condition: service_healthy     # Waits for PostgreSQL to be ready!
    networks:
      - backend_network
    restart: "no"                      # Do not restart once migration finishes

  # 3. Web API Backend (Starts only after DB is healthy and migrations finish)
  api-service:
    image: mycompany/backend-api:v1.2.0
    container_name: production_api
    environment:
      DATABASE_URL: postgresql://postgres_admin@postgres-db:5432/app_production
      PORT: "8080"
    ports:
      - "8080:8080"
    depends_on:
      postgres-db:
        condition: service_healthy     # Database must be accepting queries
      db-migration:
        condition: service_completed_successfully # Schema must be migrated
    networks:
      - backend_network
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 10s
      timeout: 3s
      retries: 3
      start_period: 5s

secrets:
  db_password:
    file: ./secrets/db_password.txt

volumes:
  postgres_data:
    driver: local

networks:
  backend_network:
    driver: bridge
```

---

## 🔍 5. Anatomy of a Docker Healthcheck Probe

A healthcheck definition is composed of five critical parameters that govern its lifecycle and evaluation:

| Parameter | Type / Format | Purpose and Working Mechanism |
| :--- | :--- | :--- |
| **`test`** | String or Array | The command executed inside the container to test health. Must return `0` for success or `1` for failure. |
| **`interval`** | Duration (`10s`, `1m`) | How frequently the Docker daemon triggers the healthcheck command. |
| **`timeout`** | Duration (`3s`, `10s`) | The maximum time allowed for the command to finish. If exceeded, the test counts as a failure. |
| **`retries`** | Integer (`3`, `5`) | The number of consecutive probe failures required before Docker marks the container as `unhealthy`. |
| **`start_period`** | Duration (`15s`, `30s`) | A buffer window during initial container boot where failures do not count toward the retry limit. |

### Test Syntax Options:
- **`["CMD", "curl", "-f", "http://localhost/"]`:** Executes `curl` directly as an executable without invoking a shell.
- **`["CMD-SHELL", "pg_isready -U postgres || exit 1"]`:** Invokes the container's default shell (`/bin/sh -c`), allowing pipe operations and logical evaluation.

---

## 🛡️ 6. Real-World Fallback: The Wait-For Script Pattern

While Compose v2 condition probes are the industry standard for local and single-host orchestration, some environments (such as Kubernetes pods without startup probes or third-party container engines) benefit from application-level resilience.

In this pattern, an entrypoint shell script (often called `wait-for-it.sh` or `wait-for`) executes inside the application container before the main server starts. The script polls the target network socket using `nc` (netcat) or TCP sockets until the port accepts connections:

```bash
#!/bin/sh
# wait-for-it.sh
host="$1"
shift
cmd="$@"

until nc -z -v -w30 "$host" 5432; do
  echo "Waiting for database connection at $host:5432..."
  sleep 2
done

echo "Database is reachable! Starting application..."
exec $cmd
```

---

## 📋 7. Summary and Navigation

Explicit dependency management via condition probes transforms chaotic multi-container startup sequences into deterministic, rock-solid application pipelines. By pairing native database healthchecks with `condition: service_healthy`, applications eliminate startup race conditions and achieve high operational stability.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Service Definitions, Networks, and Volumes](./02-Service-Definitions-Networks-and-Volumes.md) | [Index](../../../README.md) | [04 - Environment Variables and Secrets Management →](./04-Environment-Variables-and-Secrets-Management.md) |
