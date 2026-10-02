# 02 - The Linux Kernel Deep Dive

The **kernel** is the foundational core of the Linux operating system. It boots into memory first and retains absolute control over CPU execution, physical RAM allocation, filesystem integrity, and device I/O.

---

## 1. Monolithic Kernel vs. Microkernel Architecture

One of the most famous debates in computer science history occurred in 1992 between Andrew Tanenbaum (creator of MINIX) and Linus Torvalds regarding kernel design.

```
Monolithic Kernel (Linux):             Microkernel (MINIX, QNX, seL4):

┌───────────────────────────────┐     ┌───────────────────────────────┐
│          User Apps            │     │  Apps   │ FS Server │ Drivers │ (User Space)
└───────────────┬───────────────┘     └───┬─────────┴─────┬─────┴────┬──┘
                │ Syscall                 │ IPC           │ IPC      │
════════════════╪════════════════     ════╪═══════════════╪══════════╪═══
┌───────────────▼───────────────┐     ┌───▼───────────────▼──────────▼──┐
│  Linux Kernel (Ring 0)        │     │  Microkernel Core (Ring 0)      │
│  ├── VFS & Filesystems        │     │  ├── Inter-Process Comm (IPC)   │
│  ├── Network Stack            │     │  ├── Basic Scheduling          │
│  ├── CPU Scheduler            │     │  └── Low-Level Memory Handling  │
│  ├── Memory Management        │     └─────────────────────────────────┘
│  └── All Device Drivers       │
└───────────────────────────────┘
```

### Architectural Comparison:

| Feature | Monolithic Kernel (Linux) | Microkernel (e.g., MINIX, QNX) |
|---|---|---|
| **Execution Space** | All core services and device drivers run in privileged **Ring 0**. | Only minimal code runs in Ring 0; drivers and filesystems run as user space servers. |
| **Performance** | **Blazing fast.** Subsystems communicate via direct internal C function calls without context switches. | Slower due to constant Inter-Process Communication (IPC) context switching between servers. |
| **Fault Isolation** | A crash in a third-party kernel driver can crash the entire system (**Kernel Panic**). | If a filesystem driver crashes, it can be restarted as a user process without halting the OS. |
| **Linux's Pragmatic Solution** | **Modular Monolithic:** All code runs in Ring 0, but features can be compiled as dynamic **Loadable Kernel Modules (LKMs)** loaded into RAM on demand. | Strict microkernel separation. |

---

## 2. The 5 Core Subsystems of the Linux Kernel

```
                           ┌──────────────────────────┐
                           │    Syscall Interface     │
                           └────────────┬─────────────┘
                                        │
           ┌────────────────────────────┼────────────────────────────┐
           ▼                            ▼                            ▼
┌──────────────────────┐     ┌──────────────────────┐     ┌──────────────────────┐
│  Process Management  │     │  Memory Management   │     │ Virtual Filesystem   │
│  • task_struct       │     │  • Virtual Memory    │     │  • VFS Abstraction   │
│  • CFS Scheduler     │     │  • Page Allocator    │     │  • Inodes & Dentries │
│  • Namespaces/Cgroups│     │  • Page Cache / OOM  │     │  • ext4, xfs, nfs    │
└──────────┬───────────┘     └──────────┬───────────┘     └──────────┬───────────┘
           │                            │                            │
           └────────────────────────────┼────────────────────────────┘
                                        ▼
                         ┌──────────────────────────────┐
                         │ Networking & Device Drivers  │
                         │ • TCP/IP Stack & Sockets     │
                         │ • Block & Character Drivers  │
                         │ • Netfilter / eBPF Engine    │
                         └──────────────┬───────────────┘
                                        ▼
                                 [ Hardware ]
```

### 1. Process Management & Scheduler
- Represents every running thread in the system using the `task_struct` C structure.
- Employs the **Completely Fair Scheduler (CFS)** (and recently EEVDF) to allocate CPU runtime slices fairly across competing threads based on priority and "virtual runtime" (`vruntime`).
- Implements **Namespaces** (PID, Mount, Net, IPC, UTS, User) and **Control Groups (cgroups)**, which form the technical foundation of Docker and Kubernetes containers!

### 2. Memory Management (MM)
- Implements **Virtual Memory**: Every process gets its own flat 64-bit virtual address space, isolated from other processes.
- Translates virtual addresses to physical RAM via the hardware **Memory Management Unit (MMU)** using 4 KB memory **pages**.
- Manages the **Page Cache**: Caches recently read and written disk blocks in spare RAM to make subsequent file access instantaneous.
- Operates the **Out-Of-Memory (OOM) Killer**: When physical RAM and swap are exhausted, the kernel calculates `oom_score` and terminates the highest-consuming rogue process to keep the system alive.

### 3. Virtual Filesystem (VFS)
- Provides standard POSIX file operations (`open`, `read`, `write`, `close`, `stat`) across hundreds of different filesystem drivers (ext4, XFS, Btrfs, NFS, procfs, sysfs).
- Maintains the **dentry cache (dcache)** to keep file paths and inode lookups cached in memory.

### 4. Network Stack
- Implements full socket abstractions and the standard layered TCP/IP stack.
- Manages network packet buffers (`sk_buff`) from the NIC ring buffer up to userland sockets.
- Houses **Netfilter** (the packet filtering engine behind `iptables` and `nftables`) and **eBPF** (in-kernel programmable bytecode for high-performance networking and observability).

### 5. Device Drivers
- Encapsulates hardware-specific register reads and writes behind standard device node interfaces in `/dev`.
- Divides drivers into **Block devices** (storage: disks, NVMe), **Character devices** (serial streams: consoles, random generators), and **Network devices** (NIC interfaces).

---

## 3. Loadable Kernel Modules (LKMs)

Instead of requiring a full kernel recompile every time new hardware is attached, the Linux kernel dynamically loads binary object files with the `.ko` (Kernel Object) extension.

### Essential LKM Management Commands:
```bash
# 1. List all currently loaded kernel modules:
$ lsmod | head -n 10
Module                  Size  Used by
overlay               151552  12           <-- Used by Docker!
ext4                  999424  1
xfs                  2166784  0

# 2. Inspect metadata, author, and parameters of a module:
$ modinfo overlay
filename:       /lib/modules/6.5.0-35-generic/kernel/fs/overlayfs/overlay.ko
description:    Overlay filesystem
author:         Miklos Szeredi <miklos@szeredi.hu>
license:        GPL

# 3. Load a module into the running kernel along with its dependencies:
$ sudo modprobe dummy

# 4. Safely unload a module:
$ sudo modprobe -r dummy
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Architecture Basics](./01-Architecture-Basics.md) | [README](./README.md) | [03 - System Calls and Libraries](./03-System-Calls-and-Libraries.md) |
