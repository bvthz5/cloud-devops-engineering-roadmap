# 02-OS and Kernel Fundamentals

> Operating system architecture, kernel responsibilities, User Space vs Kernel Space, CPU Rings, System Calls, Process & Thread mechanics, Virtual Memory, File Descriptors, Daemons, and Security Boundaries.

---

## 🎯 Learning Objectives

By the end of this module, you will understand:
1. **OS Architecture & Responsibilities:** CPU scheduling, memory management, process management, filesystem abstraction, and I/O handling.
2. **Kernel Architectures:** Monolithic vs Microkernel vs Hybrid kernels; Ring 0 (Privileged Kernel Space) vs Ring 3 (User Space).
3. **System Calls (Syscalls):** Interface between application runtimes and the kernel (`fork`, `execve`, `read`, `write`, `socket`, `open`, `close`).
4. **Processes & Threads:** Process ID (PID), PID 1 (`init`/`systemd`), process states (Running, Sleeping, Zombie, Stopped), context switching, and thread concurrency.
5. **Memory Management & OOM Killer:** Page tables, swap memory, virtual address spaces, page faults, and Out-Of-Memory (OOM) Killer behavior.
6. **File Descriptors & Standard Streams:** File descriptor table, `0` (stdin), `1` (stdout), `2` (stderr), sockets, and pipes.
7. **Services, Drivers & Modules:** Daemons (`systemd`), Kernel modules (`lsmod`, `modprobe`), device drivers, and kernel security boundaries (Namespaces & Cgroups).

---

## 📁 Module Navigation

- [`01-Basics.md`](01-Basics.md) — Comprehensive guide to OS, Kernel, Syscalls, Processes, Threads, Memory, and File Descriptors.
- [`04-Practical-Examples.md`](04-Practical-Examples.md) — Hands-on CLI commands (`ps`, `top`, `htop`, `strace`, `lsof`, `kill`, `/proc`).
