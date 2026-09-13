# 13 - Related Topics & DevOps Progression

Mastering Linux system architecture provides the theoretical and practical foundation for every higher-level DevOps and Cloud domain.

---

## 🗺️ Architectural Learning Tree

```
                       ┌──────────────────────────────────────────────┐
                       │  02-Linux-Architecture (Current Module)      │
                       └──────────────────────┬───────────────────────┘
                                              │
             ┌────────────────────────────────┼────────────────────────────────┐
             ▼                                ▼                                ▼
┌─────────────────────────┐      ┌─────────────────────────┐      ┌─────────────────────────┐
│ 02 - Process Lifecycle  │      │ 03 - Memory & Paging    │      │ 04 - Filesystem & VFS   │
│ • fork(), exec(), wait()│      │ • Virtual Memory & MMU  │      │ • Inodes & Superblocks  │
│ • Zombie & Orphan procs │      │ • 4KB Paging, Swap, OOM │      │ • Page Cache, ext4/XFS  │
│ • Signals (SIGKILL/TERM)│      │ • Anon Memory vs Cache  │      │ • FHS Directory Tree    │
└────────────┬────────────┘      └────────────┬────────────┘      └────────────┬────────────┘
             │                                │                                │
             └────────────────────────────────┼────────────────────────────────┘
                                              ▼
                               ┌─────────────────────────────┐
                               │ 05 - Container Architecture │
                               │ • Linux Namespaces (Mount,  │
                               │   PID, Net, IPC, UTS, User) │
                               │ • Control Groups (cgroups v2│
                               │ • OverlayFS & Chroot        │
                               │ • Container Runtimes (runc) │
                               └─────────────────────────────┘
```

---

## 🔗 Deeply Connected Architectural Modules

### 1. Process Lifecycle & State Machine
- **Why it connects:** Process creation begins with the `clone()`/`fork()` and `execve()` system calls discussed in Layer 2 and Layer 4.
- **Key Concepts:** Process states (`R`, `S`, `D`, `Z`, `T`), process priorities (nice values `-20` to `+19`), and inter-process signals.

### 2. Virtual Memory, Paging & Swap
- **Why it connects:** The CPU's Memory Management Unit (MMU) in Layer 1 collaborates with the kernel's memory subsystem in Layer 2 to map 64-bit virtual memory pages to physical RAM frames.
- **Key Concepts:** Anonymous memory, dirty page flushing, memory compaction, and swap swappiness parameters.

### 3. Container Internals (Docker & Kubernetes)
- **Why it connects:** Containers are not virtual machines; they are ordinary Linux processes executing in User Space (Layer 5) isolated by kernel primitives in Layer 2:
  - **Namespaces:** Restrict what a process can *see* (its own PID list, network interfaces, and mount tree).
  - **Cgroups:** Restrict what a process can *use* (CPU shares, RAM limits, I/O bandwidth).

---

## 📚 Recommended Authoritative Reading
1. **Robert Love:** *Linux Kernel Development* (3rd Edition) — The definitive overview of kernel subsystems and design.
2. **Michael Kerrisk:** *The Linux Programming Interface (TLPI)* — The encyclopedia of Linux system calls and glibc functions.
3. **Brendan Gregg:** *Systems Performance: Enterprise and the Cloud* (2nd Edition) — Masterclass on performance profiling, `vmstat`, `perf`, and eBPF.
