# 03 - The Linux Kernel Core

The **Linux Kernel** is the central component of the operating system operating in privileged **Ring 0**. It manages physical hardware resources and allocates them fairly among competing software processes.

---

## ⚙️ Core Subsystems of the Linux Kernel

```text
               +----------------------------------------+
               |        SYSTEM CALL INTERFACE           |
               +----------------------------------------+
               | Process Scheduler  |  Memory Manager   |
               | (Completely Fair)  |  (Paging & MMU)   |
               +----------------------------------------+
               | Virtual Filesystem | Networking Stack  |
               | (VFS - Ext4/XFS)   | (TCP/IP, Sockets) |
               +----------------------------------------+
               | Device Driver Framework & IPC Layer    |
               +----------------------------------------+
```

### 1. Process Management & Scheduler
- Manages process creation (`fork()`, `execve()`), termination (`exit()`), and lifecycle states.
- Utilizes the **Completely Fair Scheduler (CFS)** to allocate CPU time slices dynamically to active threads.

### 2. Memory Management (MMU & Paging)
- Implements Virtual Memory, providing every process with its own isolated virtual address space.
- Manages physical RAM allocations, page tables, swap space, and Out-Of-Memory (OOM) killer routines.

### 3. Virtual Filesystem (VFS)
- Provides a unified abstraction interface over concrete filesystems (Ext4, XFS, Btrfs, NFS, procfs).
- Translates standardized system calls like `open()` and `write()` into specific disk driver block reads/writes.

### 4. Network Stack
- Implements network protocols (Ethernet, IPv4, IPv6, TCP, UDP, ICMP).
- Provides socket abstractions (`socket()`, `bind()`, `listen()`) for network communication.

---

## ⬅️ Navigation
- Previous: [02 - Hardware Layer](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/02-Hardware-Layer.md)
- Next: [04 - Device Drivers](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/04-Device-Drivers.md)
