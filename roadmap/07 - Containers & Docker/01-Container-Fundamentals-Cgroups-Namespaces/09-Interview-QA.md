# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the fundamental difference between a virtual machine and a container?
**Answer**: A virtual machine virtualizes hardware through a hypervisor, requiring a complete guest operating system kernel, device drivers, and heavy memory overhead. A container is a standard user-space process running directly on the host Linux kernel, isolated via kernel namespaces and resource-throttled via cgroups.

### Q2: Why is `pivot_root` preferred over `chroot` in container runtimes?
**Answer**: `chroot` only changes the apparent root directory for path resolution; root processes can easily escape chroot jails by referencing unclosed outer file descriptors. `pivot_root` atomically swaps the root filesystem in an isolated mount namespace and unmounts the old root, making it impossible to traverse back to the host filesystem.

### Q3: What happens when a container reaches its `memory.max` limit under cgroups v2?
**Answer**: The Linux kernel invokes the Out-Of-Memory (OOM) Killer. The kernel inspects `oom_score_adj` and sends `SIGKILL` (Exit Code 137) to terminate the offending process, preventing host instability.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
