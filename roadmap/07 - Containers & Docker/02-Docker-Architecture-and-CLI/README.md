# 02 - Docker Architecture and CLI

Docker revolutionized application delivery by packaging software and its dependencies into standardized, reproducible container units. This module covers Docker Engine architecture, the `dockerd` daemon, OCI runtime handoff (`containerd` and `runc`), the complete container lifecycle, and essential production CLI commands.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Docker Engine Architecture & Subsystems](./01-Docker-Engine-Architecture-and-Subsystems.md) | Client-Server REST API, `dockerd`, `containerd`, `runc`, shim processes. |
| 02 | [Container Lifecycle & Process States](./02-Container-Lifecycle-and-Process-States.md) | Lifecycle: `created`, `running`, `paused`, `restarting`, `exited`, `dead`. |
| 03 | [Essential Docker CLI Commands](./03-Essential-Docker-CLI-Commands.md) | `run`, `exec`, `logs`, `inspect`, `cp`, `commit`, `stats`, `events`, `system prune`. |
| 04 | [Resource Constraints & Limits](./04-Resource-Constraints-and-Limits.md) | Throttling CPU (`--cpus`), Memory (`--memory`), Swappiness, and OOM Kill prevention. |
| 05 | [Daemon Configuration & Production Tuning](./05-Daemon-Configuration-and-Production-Tuning.md) | `/etc/docker/daemon.json`, log drivers (`json-file`, `journald`), live-restore, default-ulimits. |
| 06 | [Multi-Architecture Builds with Buildx](./06-Multi-Architecture-Builds-with-Buildx.md) | Building for `linux/amd64` and `linux/arm64` using QEMU and Docker Buildx. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Unrotated Docker log filling root disk, dockerd restart dropping containers. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging daemon crashes, orphaned shim processes, socket permission denied. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on Docker architecture and container isolation. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Production daemon.json setup; Lab 2: Multi-arch build with Buildx; Lab 3: Log driver tuning. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for Docker CLI commands, run flags, and daemon settings. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Container Fundamentals](../01-Container-Fundamentals-Cgroups-Namespaces/README.md) | [README](./README.md) | [01 - Docker Engine Architecture](./01-Docker-Engine-Architecture-and-Subsystems.md) |
