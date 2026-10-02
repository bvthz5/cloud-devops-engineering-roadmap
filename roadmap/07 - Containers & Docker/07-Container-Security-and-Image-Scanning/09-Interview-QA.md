# 09 - Interview Questions & Architectural Scenarios

### Q1: Why is running a container as `root` (UID 0) dangerous even if it is inside an isolated namespace?
**Answer**: By default, UID 0 inside a container maps directly to UID 0 (root) on the host operating system. If an attacker discovers a kernel privilege escalation vulnerability (like a dirty COW flaw or a container runtime escape in runc), they break out of the container directly with root privileges on the physical host.

### Q2: What security advantages does `--cap-drop=ALL` provide?
**Answer**: Dropping all Linux capabilities strips the process of traditional root capabilities (like altering network routes, loading kernel modules, or changing file ownership). The process is restricted to standard user operations, preventing an attacker from executing post-exploitation techniques even if they achieve remote code execution inside the container.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
