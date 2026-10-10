# 04 - Environment Variables and Secrets Management in Docker Compose

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to configuration management, variable interpolation, `.env` file precedence, Docker secrets architecture, and preventing credential leakage in Docker Compose.

---

## 🏛️ 1. What Is Configuration Management in Containers?

The third factor of the canonical **Twelve-Factor App** methodology states: *Store config in the environment*. 

In modern cloud engineering, application code must remain strictly decoupled from environment-specific configuration values such as database hostnames, API endpoints, log levels, and authentication credentials. An image built once in a continuous integration pipeline must be able to run unchanged across local development, quality assurance (QA), staging, and production environments.

Docker Compose provides multiple mechanisms to inject configuration parameters dynamically at runtime, ranging from standard shell environment variables to cryptographically isolated Docker secrets.

---

## ⚙️ 2. Environment Variable Resolution and Precedence

Docker Compose evaluates environment variables using a strictly defined resolution order. Understanding this hierarchy prevents subtle bugs caused by unexpected configuration overrides.

```text
┌────────────────────────────────────────────────────────────────────────┐
│             VARIABLE PRECEDENCE HIERARCHY (HIGHEST TO LOWEST)          │
└────────────────────────────────────────────────────────────────────────┘

 [1. CLI Arguments]           docker compose run -e DATABASE_URL=...
         │
         ▼
 [2. Shell Environment]       export DATABASE_URL=... in host terminal
         │
         ▼
 [3. Compose 'environment:']  environment: block declared in compose.yaml
         │
         ▼
 [4. Environment Files]       env_file: [ .env.production ] in compose.yaml
         │
         ▼
 [5. Default .env File]       Default .env file in project root folder
         │
         ▼
 [6. Dockerfile Defaults]     ENV DATABASE_URL=... inside container image
```

### Detailed Hierarchy Breakdown:
1. **Command Line Overrides (`docker compose run -e ...`):** Values passed explicitly via the CLI have the highest precedence and override all file declarations.
2. **Host Shell Environment Variables:** Any variable actively exported in the host terminal shell where `docker compose` is executed overrides values in local `.env` files.
3. **The `environment:` Directive in `compose.yaml`:** Explicit declarations inside a service block override files loaded via `env_file`.
4. **Custom Environment Files (`env_file:`):** Files designated via the `env_file:` directive inject collections of key-value pairs into the container.
5. **The Default `.env` File:** Compose automatically reads a file named `.env` located in the same directory as `compose.yaml`. This file is used primarily for **interpolation** within the YAML structure itself.
6. **Container Image Defaults (`ENV` in Dockerfile):** Hardcoded fallback variables defined inside the Dockerfile have the lowest precedence.

---

## 🔬 3. String Interpolation Syntax and Default Values

Compose supports robust string interpolation syntax within YAML files, allowing developers to set sensible defaults and enforce mandatory configuration variables:

| Interpolation Syntax | Behavior and Practical Use Case |
| :--- | :--- |
| `${VARIABLE}` | Direct substitution. Replaces with the value of `VARIABLE`. If unset, substitutes an empty string. |
| `${VARIABLE:-default}` | Fallback default. Uses `VARIABLE` if set and non-empty; otherwise substitutes `default`. Perfect for local development defaults. |
| `${VARIABLE-default}` | Unset default. Uses `VARIABLE` if set (even if empty); otherwise substitutes `default`. |
| `${VARIABLE:?error_message}` | Mandatory validation. If `VARIABLE` is unset or empty, Compose aborts execution immediately with `error_message`. Prevents starting without required keys. |
| `${VARIABLE:+alternate}` | Conditional alternate. Substitutes `alternate` only if `VARIABLE` is set and non-empty. |

### Example Demonstration:
```yaml
services:
  api:
    image: mycompany/api:${TAG:-latest}           # Defaults to 'latest' if TAG is unset
    ports:
      - "${API_PORT:-8080}:8080"                  # Defaults host port to 8080
    environment:
      # Execution halts with an error if API_KEY is missing from the environment!
      API_KEY: ${API_KEY:?Error: API_KEY must be provided to run api service}
```

---

## 🔒 4. Docker Secrets: The Secure Alternative to Environment Variables

While environment variables are ideal for non-sensitive configuration parameters (such as `LOG_LEVEL=DEBUG` or `PORT=8080`), using them for sensitive secrets (such as database passwords, TLS private keys, and API tokens) introduces severe security vulnerabilities:

### Why Environment Variables Leak Secrets:
- **Process Table Exposure:** Any process running on the host or inside the container can inspect environment variables via `/proc/<pid>/environ`.
- **Inspection Leakage:** Running `docker inspect <container-id>` dumps all environment variables in plain text.
- **Application Crash Dumps:** Many application frameworks output the entire environment table during unhandled exceptions or error logging.
- **Child Process Inheritance:** Environment variables are inherited by all child processes spawned inside the container.

### The Docker Secrets Architecture:
Docker Secrets resolves these risks by mounting sensitive credentials into an in-memory virtual filesystem (`tmpfs`) at `/run/secrets/<secret_name>` inside the container. The secret is never written to disk, is never committed to image layers, and is never exposed via `docker inspect`.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   DOCKER SECRETS RUNTIME MOUNTING                      │
└────────────────────────────────────────────────────────────────────────┘

  Host Filesystem:
  ./secrets/db_password.txt (Restricted file permissions, e.g., 0400)
         │
         ▼ (Mounted into container via in-memory tmpfs)
  Container Filesystem:
  /run/secrets/db_password (Available only to container process memory)
```

---

## 📝 5. Production-Grade Compose Secrets Implementation

The following example illustrates how to configure a database and an application service using native Docker Secrets:

```yaml
services:
  database:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: production_db
      POSTGRES_USER: admin_user
      # Directs PostgreSQL to read password from the mounted secret file
      POSTGRES_PASSWORD_FILE: /run/secrets/db_root_password
    secrets:
      - db_root_password
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - internal_net

  application:
    image: mycompany/enterprise-api:v2.0
    environment:
      DB_HOST: database
      DB_NAME: production_db
      DB_USER: admin_user
      # The application reads /run/secrets/db_root_password directly
      DB_PASSWORD_FILE: /run/secrets/db_root_password
    secrets:
      - db_root_password
      - api_jwt_key
    networks:
      - internal_net

# Top-level secrets declaration
secrets:
  db_root_password:
    file: ./secrets/db_password.txt      # Path to local secret file on host
  api_jwt_key:
    file: ./secrets/jwt_private.pem

volumes:
  db_data:

networks:
  internal_net:
    driver: bridge
```

---

## 🛡️ 6. Security Best Practices for Secret Management

1. **Add `.env` and Secrets to `.gitignore`:** Never commit plain-text `.env` files, private keys, or passwords to source control repositories. Provide a sanitized `.env.example` file containing empty placeholders.
2. **Use File-Based Secret Handlers in Application Code:** Configure application frameworks to read credentials from files specified by `*_FILE` environment variables (e.g., checking if `process.env.DB_PASSWORD_FILE` exists before falling back to `process.env.DB_PASSWORD`).
3. **Restrict Host Permissions:** Ensure secret files on the host machine are owned by the administrator and restricted with `chmod 600` (read/write only by owner).

---

## 📋 7. Summary and Navigation

Separating non-sensitive runtime parameters into `env_file` configurations while isolating sensitive credentials inside native Docker Secrets provides a secure, robust foundation for containerized applications. This practice ensures compliance with enterprise security benchmarks while preserving developer agility across environments.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Dependency Management and Healthchecks](./03-Dependency-Management-and-Healthchecks.md) | [Index](../../../README.md) | [05 - Compose Profiles and Multi-Environment Overrides →](./05-Compose-Profiles-and-Multi-Environment-Overrides.md) |
