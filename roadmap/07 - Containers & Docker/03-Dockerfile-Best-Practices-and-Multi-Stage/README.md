# 03 - Dockerfile Best Practices and Multi-Stage

Writing efficient, secure, and production-grade Dockerfiles is a fundamental DevOps discipline. Poorly constructed Dockerfiles lead to bloated multi-gigabyte images, slow CI/CD build times, security vulnerabilities from embedded build toolchains, and cache invalidation bottlenecks. This module covers multi-stage builds, layer caching, distroless images, non-root execution, and BuildKit optimizations.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Dockerfile Instruction Anatomy & Execution](./01-Dockerfile-Instruction-Anatomy-and-Execution.md) | `FROM`, `RUN`, `COPY`, `ADD`, `WORKDIR`, `EXPOSE`, `ENV`, `ARG`, instruction ordering. |
| 02 | [Layer Caching Optimization & Build Order](./02-Layer-Caching-Optimization-and-Build-Order.md) | How Docker caches layers, invalidation triggers, ordering from least to most frequent change. |
| 03 | [Multi-Stage Builds Architecture](./03-Multi-Stage-Builds-Architecture.md) | Decoupling build environment from runtime, reducing image sizes from 1.5GB to 15MB. |
| 04 | [Distroless, Alpine & Scratch Base Images](./04-Distroless-Alpine-and-Scratch-Base-Images.md) | Trade-offs: glibc vs musl libc (Alpine bugs), Google Distroless, static Go binaries in `scratch`. |
| 05 | [Non-Root Users & Least Privilege Execution](./05-Non-Root-Users-and-Least-Privilege-Execution.md) | Creating dedicated unprivileged users (`USER appuser`), file permission boundaries. |
| 06 | [BuildKit Advanced Features & Cache Mounts](./06-BuildKit-Advanced-Features-and-Cache-Mounts.md) | `DOCKER_BUILDKIT=1`, `--mount=type=cache` for pip/go/npm, secret mounts (`--mount=type=secret`). |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Leaked AWS credentials in build layer, Alpine musl DNS resolution bug. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Diagnosing broken cache hits, inspecting image layers with `dive`, debugging BuildKit syntax. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on Dockerfile optimization, multi-stage, and security. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Production Multi-stage Go Dockerfile; Lab 2: Python Distroless build; Lab 3: Dive inspection. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for Dockerfile instructions, layer caching rules, and BuildKit syntax. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Docker Architecture](../02-Docker-Architecture-and-CLI/README.md) | [README](./README.md) | [01 - Dockerfile Instruction Anatomy](./01-Dockerfile-Instruction-Anatomy-and-Execution.md) |
