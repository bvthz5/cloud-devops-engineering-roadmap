# 03 - Multi-Stage Builds Architecture

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to Docker multi-stage builds, decoupling development SDKs from production runtimes, cross-stage artifact copying, parallel BuildKit stage execution, and reducing image attack surfaces.

---

## 🏛️ 1. What Are Multi-Stage Builds & Why Were They Invented?

In modern software development, building and running applications require completely contradictory environments:
- **Build-Time Requirements:** Software compilation requires heavy development kits, compilers (such as `gcc`, `g++`, `clang`), language SDKs (such as Go SDK, Rust toolchain, Node.js, JDK), header files, package managers, and version control tools (`git`). These dependencies routinely inflate intermediate environments to **1.5 GB to 2.5 GB** in size.
- **Production Runtime Requirements:** The production environment requires **only** the final compiled executable binary or transpiled assets, shared runtime libraries, and a secure configuration. Compilers, git repositories, and build tools have no operational purpose in production.

Leaving build toolchains inside production images introduces severe problems:
1. **Massive Attack Surface:** Compilers and shell utilities left inside a container allow an attacker who discovers an application vulnerability to compile exploits directly on the production host.
2. **Endless CVE Vulnerabilities:** Every unused operating system package and compiler library triggers false-positive and critical CVE security scanner alerts (such as Trivy or Grype).
3. **Slow Deployment Velocity:** Transferring gigabyte-sized images across container registries and orchestrators consumes enormous network bandwidth and creates multi-minute deployment delays.

### The Historical "Builder Pattern" Antipattern:
Before Docker introduced native multi-stage builds in version 17.05, engineering teams had to maintain two separate Dockerfiles: a `Dockerfile.build` containing compilers, and a `Dockerfile` for production. A complex Bash script built the first image, launched an ephemeral container, executed `docker cp` to extract the binary to the host machine, and copied the binary into the second image. This workflow was fragile, error-prone, and required custom CI orchestration.

**Docker Multi-Stage Builds** solved this problem permanently by allowing multiple `FROM` instructions within a single, unified Dockerfile.

---

## ⚙️ 2. How Multi-Stage Builds Work Internally

A multi-stage Dockerfile defines multiple distinct build stages sequentially. Each `FROM` instruction begins a completely isolated build environment with its own base operating system and filesystem.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   MULTI-STAGE BUILD EXECUTION FLOW                     │
└────────────────────────────────────────────────────────────────────────┘

 Stage 1: Build Environment (Temporary)
 ┌──────────────────────────────────────────────────────────────────────┐
 │ FROM golang:1.22-alpine AS builder                                   │
 │ - Downloads full Go compiler & toolchain (350 MB)                    │
 │ - Downloads source dependencies (go mod download)                    │
 │ - Compiles static binary: CGO_ENABLED=0 go build -o /bin/api         │
 └──────────────────────────────────┬───────────────────────────────────┘
                                    │
                                    │ COPY --from=builder /bin/api /app/api
                                    │ (Copies ONLY the compiled 15MB binary!)
                                    ▼
 Stage 2: Production Runtime (Final Immutable Image)
 ┌──────────────────────────────────────────────────────────────────────┐
 │ FROM gcr.io/distroless/static-debian12                               │
 │ - Contains ZERO compilers, ZERO package managers, ZERO shells       │
 │ - Image size: 18 MB Total!                                           │
 │ - Discards Stage 1 layers completely from the final manifest!        │
 └──────────────────────────────────────────────────────────────────────┘
```

### Key Working Principles:
1. **Named Stages (`AS <stage-name>`):** Developers assign human-readable aliases to stages using the `AS` keyword (e.g., `FROM node:20-alpine AS builder`).
2. **Cross-Stage File Transfer (`COPY --from`):** The `COPY` instruction accepts a `--from=<stage-name>` flag that allows you to copy specific files or directories directly from an earlier stage into the current stage's filesystem.
3. **Automatic Intermediate Discard:** When the final stage completes, Docker publishes only the layers belonging to the **last stage** as the final image tag. All intermediate compiler layers, SDKs, and source code from earlier stages are completely discarded from the final image manifest.
4. **Parallel BuildKit Execution:** Under the modern BuildKit engine, independent build stages that do not rely on each other are executed **concurrently in parallel**, dramatically reducing total compilation time. Furthermore, if a stage is never referenced by downstream stages or the build target, BuildKit prunes it completely and skips its execution.

---

## 🔬 3. Multi-Stage Architectures Across Major Languages

The following real-world examples demonstrate how multi-stage builds are implemented across diverse language ecosystems:

### 3.1 Compiled Static Languages (Go / Rust)
Static languages produce standalone machine binaries with zero external dependencies, allowing the final runtime to run on a minimal `scratch` or `distroless` base:

```dockerfile
# Stage 1: Compile Go Application
FROM golang:1.22-bookworm AS builder
WORKDIR /workspace
COPY go.mod go.sum ./
RUN go mod download
COPY . .
# Compile statically linked binary with debugging symbols stripped (-w -s)
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o /bin/microservice .

# Stage 2: Scratch Minimal Runtime (Total Size: ~15 MB!)
FROM scratch
WORKDIR /
# Copy SSL root certificates for HTTPS network requests
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /bin/microservice /microservice
USER 65534:65534
ENTRYPOINT ["/microservice"]
```

### 3.2 Single-Page Web Applications (React / Vue / Angular)
Web applications require heavy Node.js environments and NPM dependencies to transpile TypeScript and bundle HTML/CSS/JS, but runtime hosting requires only a high-performance web server:

```dockerfile
# Stage 1: Build & Bundle Web Assets
FROM node:20-alpine AS bundler
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build   # Generates compiled static assets in /app/dist/

# Stage 2: High-Performance Production Web Server
FROM nginx:1.25-alpine
# Replace default Nginx configuration with optimized SPA config
COPY nginx.conf /etc/nginx/conf.d/default.conf
# Copy ONLY the static HTML, CSS, and JS files from the bundler stage
COPY --from=bundler /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 3.3 Interpreted Languages with C-Extensions (Python)
Python applications often require compilers to build native wheels (such as `psycopg2` or `numpy`), but those compilers should never reach production:

```dockerfile
# Stage 1: Wheel Builder
FROM python:3.11-slim-bookworm AS builder
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*
WORKDIR /install
COPY requirements.txt .
# Compile and install dependencies into a localized wheel folder
RUN pip install --no-cache-dir --prefix=/install/local -r requirements.txt

# Stage 2: Clean Production Runtime
FROM python:3.11-slim-bookworm AS runtime
RUN apt-get update && apt-get install -y --no-install-recommends \
    libpq5 \
    && rm -rf /var/lib/apt/lists/*
WORKDIR /app
# Copy only the compiled Python packages from the builder stage
COPY --from=builder /install/local /usr/local
COPY . /app
USER 1000
CMD ["python", "app.py"]
```

---

## 🎯 4. Targeting Specific Stages for Testing and Development

Multi-stage Dockerfiles can serve as an entire continuous integration workflow by defining dedicated testing and linting stages that can be targeted on-demand using the `--target` flag:

```dockerfile
FROM node:20-alpine AS base
WORKDIR /app
COPY package*.json ./
RUN npm ci

# Intermediate Stage: Unit Tests & Code Linting
FROM base AS test
COPY . .
RUN npm run lint
RUN npm run test:unit

# Production Compilation Stage
FROM base AS builder
COPY . .
RUN npm run build

# Final Production Stage
FROM node:20-alpine AS release
WORKDIR /app
COPY --from=builder /app/dist ./dist
USER node
CMD ["node", "dist/index.js"]
```

### Executing Target Builds:
```bash
# Runs the linter and unit tests in CI; halts build if tests fail!
docker build --target test -t myapp:test .

# Builds the lean production release image
docker build --target release -t myapp:prod .
```

---

## 📦 5. Copying Artifacts from External Container Images

The `--from` flag is not restricted solely to stages defined earlier within the same Dockerfile. You can directly reference any arbitrary external container image from Docker Hub or a private registry.

This provides an ultra-clean method to install pre-compiled CLI utilities (such as `trivy`, `helm`, or `uv`) without needing to install package managers, git, or curl:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

# Copy the high-performance 'uv' Python package manager from its official image!
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

# Copy Docker CLI binary from official Docker image
COPY --from=docker:26-cli /usr/local/bin/docker /usr/local/bin/docker

COPY pyproject.toml .
RUN uv pip install --system -r pyproject.toml
```

---

## 📋 6. Summary and Navigation

Multi-stage builds are an indispensable architectural standard for production Dockerfiles. By strictly separating compilation toolchains from runtime containers, development teams produce secure, hardened images with minimal attack surfaces, zero compiler footprints, and lightning-fast deployment times.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Layer Caching Optimization and Build Order](./02-Layer-Caching-Optimization-and-Build-Order.md) | [Index](../../../README.md) | [04 - Distroless Alpine and Scratch Base Images →](./04-Distroless-Alpine-and-Scratch-Base-Images.md) |
