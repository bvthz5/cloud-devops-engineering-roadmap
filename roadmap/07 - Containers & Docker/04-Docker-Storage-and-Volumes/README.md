# 04 - Docker Storage and Volumes

Containers are ephemeral by default; when a container is deleted, all data written to its writable container layer is lost permanently. For databases, stateful caches, and logs, Docker provides structured persistent storage mechanisms: **Named Volumes**, **Bind Mounts**, and in-memory **tmpfs** mounts. Understanding their performance, permissions, and underlying storage driver mechanics (`overlay2`) is essential for production operations.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Storage Drivers & Overlay2 Deep Dive](./01-Storage-Drivers-and-Overlay2-Deep-Dive.md) | `overlay2` architecture, `lowerdir`, `upperdir`, `merged`, `workdir`, inode consumption. |
| 02 | [Named Volumes vs Bind Mounts vs tmpfs](./02-Named-Volumes-vs-Bind-Mounts-vs-tmpfs.md) | Comparison matrix, host decoupling, permission handling, macOS/Windows performance. |
| 03 | [Volume Lifecycle, Backup & Restoration](./03-Volume-Lifecycle-Backup-and-Restoration.md) | Creating, inspecting, pruning volumes, backup container pattern, tar streaming. |
| 04 | [Tmpfs Mounts & In-Memory Security](./04-Tmpfs-Mounts-and-In-Memory-Security.md) | Storing sensitive tokens and ephemeral state in RAM, size limits, security isolation. |
| 05 | [External Volume Plugins & Cloud Storage](./05-External-Volume-Plugins-and-Cloud-Storage.md) | Docker volume plugins, NFS, AWS EFS integration, distributed persistent storage. |
| 06 | [Storage Optimization & Dangling Cleanup](./06-Storage-Optimization-and-Dangling-Cleanup.md) | Reclaiming disk space, removing orphaned volumes, tracking container disk churn. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Inode exhaustion crash, host bind mount permission overwrite. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging permission denied on volumes, inspecting `/var/lib/docker/volumes/`. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on storage drivers, volumes, and performance. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Persistent PostgreSQL with Named Volume; Lab 2: Automated backup pipeline; Lab 3: tmpfs. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for volume CLI commands, mount syntax (`-v` vs `--mount`), and flags. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Dockerfile Best Practices](../03-Dockerfile-Best-Practices-and-Multi-Stage/README.md) | [README](./README.md) | [01 - Storage Drivers & Overlay2](./01-Storage-Drivers-and-Overlay2-Deep-Dive.md) |
