# 06 - BuildKit Advanced Features and Cache Mounts

## 1. Cache Mounts: Fast Incremental Builds

Under standard Docker builds, package manager caches are wiped between layers.
With **BuildKit cache mounts (`--mount=type=cache`)**, compiler and package caches persist across builds without bloating the image layer:

```dockerfile
# syntax=docker/dockerfile:1.4
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .

# Cache pip downloads in BuildKit cache directory!
RUN --mount=type=cache,target=/root/.cache/pip \
    pip install -r requirements.txt
```

---

## 2. Secret Mounts: Zero-Leak Credentials

Never pass API keys or Git tokens via `ARG` or `ENV` (they get baked permanently into image layer history!). Use **Secret Mounts**:

```dockerfile
# syntax=docker/dockerfile:1.4
FROM alpine
# Mounts secret into /run/secrets/npm_token ONLY for this command execution!
RUN --mount=type=secret,id=npm_token \
    NPM_TOKEN=$(cat /run/secrets/npm_token) npm publish
```

```bash
docker build --secret id=npm_token,src=.npmrc .
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Non-Root Users & Least Privilege](./05-Non-Root-Users-and-Least-Privilege-Execution.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
