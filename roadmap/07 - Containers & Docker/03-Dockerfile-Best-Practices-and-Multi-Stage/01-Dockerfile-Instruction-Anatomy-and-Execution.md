# 01 - Dockerfile Instruction Anatomy and Execution

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to the Dockerfile syntax, instruction execution mechanics, build engine caching, signal handling, PID 1 behavior, and production authoring standards.

---

## 🏛️ 1. What Is a Dockerfile & How Does Docker Build Images?

A **Dockerfile** is a declarative, line-by-line configuration script that instructs the Docker build engine (or modern BuildKit engine) how to assemble a standalone, runnable container image. Every line in a Dockerfile consists of an instruction verb in uppercase followed by arguments (e.g., `FROM node:20-alpine`, `COPY . /app`).

When an engineer issues `docker build -t myapp .`, the Docker client packages the specified directory (the **build context**) and transmits it to the Docker daemon. The build engine processes the Dockerfile sequentially from top to bottom.

Under classic Docker builds, each instruction that modifies the filesystem (such as `RUN`, `COPY`, and `ADD`) creates an intermediate container, executes the command, saves the modified filesystem state as a new immutable read-only image layer, and destroys the temporary container. Under modern **BuildKit**, instructions are analyzed into an intermediate Directed Acyclic Graph (DAG) called **LLB (Low-Level Builder)**, allowing independent build stages to execute concurrently in parallel and caching layers with sub-millisecond precision.

```text
┌────────────────────────────────────────────────────────────────────────┐
│               DOCKERFILE LAYERED BUILD EXECUTION MODEL                 │
└────────────────────────────────────────────────────────────────────────┘

  Dockerfile Instructions                  OCI Image Layers (Read-Only)
  ───────────────────────                  ────────────────────────────
  FROM alpine:3.19              ───►  Layer 1: [ Base Alpine OS Filesystem ]
  RUN apk add --no-cache nodejs ───►  Layer 2: [ Node.js Binaries & Libs   ]
  WORKDIR /app                  ───►  Metadata: Working directory set to /app
  COPY package.json .           ───►  Layer 3: [ package.json added        ]
  RUN npm install               ───►  Layer 4: [ node_modules directory    ]
  COPY . .                      ───►  Layer 5: [ Application source code   ]
  CMD ["node", "server.js"]     ───►  Metadata: Default runtime command
```

---

## ⚙️ 2. Comprehensive Anatomy of Every Dockerfile Instruction

The following catalog provides complete, sentence-by-sentence explanations of every core instruction supported by the OCI and Docker image specification:

### 2.1 `FROM` (Image Foundation)
- **What it is:** The mandatory opening instruction of any valid Dockerfile that initializes a new build stage and establishes the base image.
- **Working Mechanism:** The build engine queries local storage for the specified image repository and tag. If not found locally, it contacts the remote container registry (such as Docker Hub) to download the base filesystem layers.
- **Syntax:** `FROM <image>[:<tag>] [AS <stage-name>]`
- **Example:** `FROM python:3.12-slim-bookworm AS builder`

### 2.2 `RUN` (State Transformation)
- **What it is:** The primary instruction used to execute commands, install operating system packages, compile code, and create directories during image assembly.
- **Working Mechanism:** Runs the specified shell command inside an ephemeral container on top of the current layer. Any filesystem modifications made by the command are committed as a new permanent image layer.
- **Best Practice:** Always chain related commands together using `&&` and purge package manager caches in the same instruction layer to avoid inflating image size:
  ```dockerfile
  RUN apt-get update && apt-get install -y --no-install-recommends \
      curl \
      ca-certificates \
      && rm -rf /var/lib/apt/lists/*
  ```

### 2.3 `COPY` vs. `ADD` (File Injection)
- **`COPY`:** Transparently copies files and directories from the host build context into the container filesystem. It supports ownership changes directly via `--chown=<user>:<group>`. It is the recommended instruction for 99% of file transfers.
  ```dockerfile
  COPY --chown=node:node src/ /app/src/
  ```
- **`ADD`:** A legacy instruction with specialized features. In addition to copying files, it can download remote files from URLs and automatically unpack compressed archives (`.tar`, `.tar.gz`, `.tar.bz2`) into the destination folder. Because downloading remote URLs via `ADD` prevents cache cleaning, industry security standards mandate using `COPY` instead, reserving `ADD` solely for auto-extracting local tarballs.

### 2.4 `WORKDIR` (Execution Directory)
- **What it is:** Sets the working directory for all subsequent `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions.
- **Working Mechanism:** If the target directory does not exist, Docker automatically creates it with root ownership.
- **Critical Rule:** Never use `RUN cd /app` because each `RUN` instruction executes in a separate subshell, meaning the directory change is discarded in the subsequent line. Always use `WORKDIR /app`.

### 2.5 `ENV` vs. `ARG` (Variable Management)
Understanding the boundary between build time and runtime is vital:

| Attribute | `ARG` (Build-Time Argument) | `ENV` (Runtime Environment Variable) |
| :--- | :--- | :--- |
| **Availability Window** | Accessible **only** during `docker build`. | Accessible during build **and** inside running container. |
| **CLI Override Flag** | `--build-arg KEY=VALUE` | `-e KEY=VALUE` or `--env-file` |
| **Layer Persistence** | Not persisted in final image metadata (except via history). | Persisted permanently into image JSON configuration. |
| **Primary Use Case** | Passing package versions, build tokens, or target architectures. | Defining application ports, database hostnames, and log levels. |

```dockerfile
ARG APP_VERSION=1.0.0      # Available only while building the image
ENV NODE_ENV=production    # Injected into the container's process environment
```

### 2.6 `EXPOSE` (Port Documentation)
- **What it is:** A documentation instruction that informs container operators and automated tooling which network ports the containerized service listens on at runtime.
- **Working Mechanism:** `EXPOSE` does **not** publish the port to the host machine or make it accessible outside Docker. Port publishing must be explicitly activated at runtime using `docker run -p 8080:80` or Compose `ports:`.

### 2.7 `USER` (Least Privilege Principle)
- **What it is:** Sets the user name or UID (and optionally group or GID) under which all subsequent instructions and the final application container process will run.
- **Working Mechanism:** By default, containers execute as the Linux superuser (`root`, UID 0). Declaring `USER <non-root-uid>` forces the Linux kernel to drop root capabilities, preventing attackers from compromising the host operating system in the event of a container breakout vulnerability.

### 2.8 `VOLUME` (Declaration of Persistence)
- **What it is:** Creates a mount point with the specified path and marks it as holding externally mounted persistent volume storage.
- **Working Mechanism:** Any data written to a declared volume directory bypasses the container's Copy-on-Write storage driver, writing directly to host-managed volume storage.

### 2.9 `HEALTHCHECK` (Readiness Validation)
- **What it is:** Configures an internal test probe to verify that the application running inside the container is functioning normally, rather than just running as an idle or hung process.
  ```dockerfile
  HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
    CMD curl -f http://localhost:8080/health || exit 1
  ```

---

## 🔬 3. `ENTRYPOINT` vs. `CMD`: The Complete Resolution Matrix

The interaction between `ENTRYPOINT` and `CMD` dictates what executable is spawned when a container starts:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   ENTRYPOINT + CMD EXECUTION MODEL                     │
└────────────────────────────────────────────────────────────────────────┘

 Dockerfile:
   ENTRYPOINT ["node", "app.js"]    ──► The fixed executable binary
   CMD ["--port", "8080"]           ──► The default arguments

 Runtime Command:
   docker run myapp                 ──► Spawns: node app.js --port 8080
   docker run myapp --port 9090     ──► Spawns: node app.js --port 9090
                                        (CLI arguments override CMD, not ENTRYPOINT)
```

### Operational Rules:
1. **`ENTRYPOINT`:** Defines the fixed binary or entrypoint shell script that should always run when the container starts. It cannot be overridden by normal CLI arguments (it requires `--entrypoint`).
2. **`CMD`:** Defines the default arguments passed to the `ENTRYPOINT`. If an engineer provides trailing arguments to `docker run` (e.g., `docker run myapp --dry-run`), those arguments replace `CMD` entirely while preserving `ENTRYPOINT`.
3. **Standalone `CMD`:** If `ENTRYPOINT` is omitted, `CMD` acts as the complete command and can be entirely replaced at runtime (e.g., `docker run -it myapp /bin/bash`).

---

## ⚠️ 4. Shell Form vs. Exec Form & The PID 1 Zombie Problem

Both `RUN`, `CMD`, and `ENTRYPOINT` support two syntax formats, but choosing the wrong form introduces catastrophic process lifecycle failures:

### The Shell Form (Antipattern for Runtime Commands):
```dockerfile
CMD node server.js
```
- **What happens internally:** Docker invokes the default system shell: `/bin/sh -c "node server.js"`.
- **The PID 1 Problem:** The shell process (`/bin/sh`) becomes **PID 1** inside the container. When you issue `docker stop`, Docker transmits a **SIGTERM** signal to PID 1. Standard POSIX shells do **not** forward signals to child processes by default. As a result, your Node.js server never receives the graceful shutdown signal, cannot flush pending database transactions or close HTTP sockets, and is abruptly killed with a **SIGKILL** (Exit Code 137) after a 10-second timeout.

### The Exec Form (Mandatory Best Practice):
```dockerfile
CMD ["node", "server.js"]
```
- **What happens internally:** Docker uses the `execve()` system call to launch `node` directly as **PID 1** without intermediate shell wrappers.
- **The Result:** The application process receives **SIGTERM** directly, enabling graceful socket termination, connection cleanup, and clean container shutdown.

---

## 📝 5. Production-Grade Enterprise Dockerfile Template

The following production Dockerfile implements every best practice: multi-stage compilation, non-root user execution, dumb-init PID 1 signal forwarding, and cache optimization:

```dockerfile
# ------------------------------------------------------------------------------
# Stage 1: Build & Compilation Environment
# ------------------------------------------------------------------------------
FROM node:20-alpine AS builder

WORKDIR /build

# Copy dependency manifests first to maximize layer cache hit rate
COPY package.json package-lock.json ./

# Install dependencies cleanly
RUN npm ci

# Copy remaining source code and compile TypeScript to JavaScript
COPY . .
RUN npm run build

# ------------------------------------------------------------------------------
# Stage 2: Hardened Production Runtime Environment
# ------------------------------------------------------------------------------
FROM node:20-alpine AS production

# Install dumb-init to properly handle PID 1 zombie reaping and signal forwarding
RUN apk add --no-cache dumb-init

ENV NODE_ENV=production
WORKDIR /usr/src/app

# Create a dedicated unprivileged user and group
RUN addgroup -g 1001 -S appgroup && \
    adduser -u 1001 -S appuser -G appgroup

# Copy compiled artifacts and production dependencies from builder stage
COPY --from=builder --chown=appuser:appgroup /build/package*.json ./
COPY --from=builder --chown=appuser:appgroup /build/node_modules ./node_modules
COPY --from=builder --chown=appuser:appgroup /build/dist ./dist

# Switch execution away from root to the unprivileged service user
USER appuser

# Expose internal application port
EXPOSE 3000

# Healthcheck probe to monitor operational responsiveness
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health || exit 1

# Launch application via dumb-init for robust signal forwarding
ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["node", "dist/server.js"]
```

---

## 📋 6. Summary and Navigation

Understanding the granular mechanics of Dockerfile instructions allows engineers to write clean, predictable, and secure container images. By adhering to the Exec form for commands, running under unprivileged user accounts, and structuring layer ordering, applications achieve rapid build times and resilient runtime behavior.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (02-Docker-Architecture-and-CLI)](../02-Docker-Architecture-and-CLI/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Layer Caching Optimization and Build Order →](./02-Layer-Caching-Optimization-and-Build-Order.md) |
