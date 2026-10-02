# 02 - Layer Caching Optimization and Build Order

## 1. How Layer Caching Works

Docker builds images layer by layer. For each instruction, Docker checks if a matching cached layer exists:
- If unchanged: Reuses the cache (`CACHED`).
- If an instruction **misses** the cache: That layer and **EVERY SUBSEQUENT LAYER** must be rebuilt from scratch!

```text
POORLY ORDERED DOCKERFILE (Rebuilds dependencies on EVERY code edit!):
FROM python:3.11-slim
WORKDIR /app
COPY . .                     <-- Changes on every git commit! Invalidates cache!
RUN pip install -r requirements.txt <-- Slow 4-minute re-download every build!

OPTIMIZED DOCKERFILE (Reuses cached dependencies!):
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .      <-- Only changes when libraries change! CACHED!
RUN pip install -r requirements.txt <-- Skipped in 0.1s!
COPY . .                     <-- Fast layer copy at the end!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Dockerfile Instruction Anatomy](./01-Dockerfile-Instruction-Anatomy-and-Execution.md) | [README](./README.md) | [03 - Multi-Stage Builds Architecture](./03-Multi-Stage-Builds-Architecture.md) |
