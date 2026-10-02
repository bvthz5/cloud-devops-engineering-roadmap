# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the difference between CPU throttling and Memory OOMKilling?
**Answer:**
CPU is a **compressible resource**. When a container attempts to use more CPU than its limit, the Linux kernel CFS quota throttles the CPU time slices allocated to the process. The application slows down, but the process continues running.
Memory is an **incompressible resource**. When a container attempts to allocate memory beyond its limit, the Linux kernel cannot "throttle" memory; the kernel OOM-killer immediately terminates the process with SIGKILL (Exit code `137`).

---

### Q2: What is the purpose of the Pause Container in every Pod?
**Answer:**
The Pause container serves two vital functions:
1. It serves as the parent container that holds the shared Linux namespaces (Network, IPC) open. When application containers restart or crash, the network IP and port bindings remain completely intact.
2. It acts as PID 1 for the Pod's PID namespace, reaping orphaned zombie child processes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
