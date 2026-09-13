# 17 - Related Topics & DevOps Progression

Operating System concepts serve as the bedrock for systems programming, container runtimes, and cloud architecture.

---

## 🗺️ The Operating System Curriculum Map

```
               ┌────────────────────────────────────────────────────────┐
               │    03-Operating-System (Core Theory & Fundamentals)    │
               └───────────────────────────┬────────────────────────────┘
                                           │
             ┌─────────────────────────────┼─────────────────────────────┐
             ▼                             ▼                             ▼
┌─────────────────────────┐   ┌─────────────────────────┐   ┌─────────────────────────┐
│ 01-Directory-Structure  │   │ 02-Linux-Architecture   │   │ 04 - Process & Concur-  │
│ • Filesystem Hierarchy  │   │ • 5-Layer Model         │   │   rency Deep Dive       │
│ • VFS, Mounts, Inodes   │   │ • Kernel Space vs User  │   │ • Multithreading, Locks │
│ • FHS 3.0 Standard      │   │ • cp System Call Trace  │   │ • Race Conditions, IPC  │
└────────────┬────────────┘   └────────────┬────────────┘   └────────────┬────────────┘
             │                             │                             │
             └─────────────────────────────┼─────────────────────────────┘
                                           ▼
                              ┌─────────────────────────┐
                              │ 05 - Cloud-Native Infra │
                              │ • Linux Cgroups & OOM   │
                              │ • Namespaces & Runtimes │
                              │ • Kubernetes Pod Specs  │
                              │ • Hypervisor vs VM vs K8s│
                              └─────────────────────────┘
```

---

## 🔗 Next Core Deep Dives

### 1. Concurrency, Locks & Synchronization
- **Why it connects:** CPU scheduling and multi-core processing require synchronization primitives to prevent race conditions when updating shared state.
- **Key Concepts:** Mutexes, Spinlocks, Semaphores, Read-Write Locks, Futexes (`Fast Userspace Mutex`).

### 2. Hypervisors & Virtualization (Type 1 vs. Type 2)
- **Why it connects:** Cloud infrastructure (AWS EC2, Google Cloud Engine) runs virtual operating systems on top of hypervisors.
- **Key Concepts:** Type 1 (Bare-Metal: KVM, VMware ESXi, AWS Nitro) vs Type 2 (Hosted: VirtualBox), CPU hardware virtualization extensions (Intel VT-x / AMD-V), Memory overcommitment, and VirtIO para-virtualized drivers.

### 3. Linux Kernel Tuning via `sysctl`
- **Why it connects:** Operating systems come with general-purpose defaults. Production web servers and databases require tuning kernel variables.
- **Key Concepts:** Tuning `vm.swappiness`, `vm.dirty_ratio`, `net.core.somaxconn`, `net.ipv4.tcp_max_syn_backlog`, and `fs.file-max`.

---

## 📚 Canonical Textbooks & Documentation
1. **Silberschatz, Galvin, Gagne:** *Operating System Concepts* ("The Dinosaur Book") — The classic computer science textbook on OS theory.
2. **Andrew S. Tanenbaum:** *Modern Operating Systems* — Detailed analysis of processes, memory, file systems, and distributed OS design.
3. **Arpaci-Dusseau:** *Operating Systems: Three Easy Pieces (OSTEP)* — Free, world-class book covering Virtualization, Concurrency, and Persistence.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - Quick Revision](./16-Quick-Revision.md) | [README](./README.md) | — |
