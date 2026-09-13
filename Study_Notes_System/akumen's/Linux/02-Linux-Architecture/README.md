# Linux Operating System Architecture

Welcome to the definitive guide on **Linux System Architecture**. This module explores how hardware, kernel subsystems, C standard libraries, system utilities, and userland applications collaborate to run modern workloads.

---

## 🏛️ The 5-Layer Architectural Model

```
┌─────────────────────────────────────────────────────────────────────────┐
│ Layer 5: User Applications & Services                                   │
│ (Nginx, Docker, Python, PostgreSQL, bash, git, curl, custom code)      │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │
┌────────────────────────────────────▼────────────────────────────────────┐
│ Layer 4: System Utilities & Shell                                       │
│ (Coreutils: cp, ls, grep, ps, systemd, sshd, package managers)          │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │
┌────────────────────────────────────▼────────────────────────────────────┐
│ Layer 3: System Libraries & C Runtime (glibc / musl)                    │
│ (POSIX API wrappers: open, read, write, fork, malloc, pthread)          │
└────────────────────────────────────┬────────────────────────────────────┘
                  ▲                  │
══════════════════╪══════════════════╪═════════════════════════════════════
  User Space      │ CPU Context      │ System Call Interface (syscall)
  (x86 Ring 3)    │ Switch           ▼ Trap / Syscall Instruction
──────────────────┼─────────────────────────────────────────────────────────
  Kernel Space    │
  (x86 Ring 0)    │
══════════════════╪═════════════════════════════════════════════════════════
┌─────────────────┴───────────────────────────────────────────────────────┐
│ Layer 2: The Linux Kernel (Monolithic with Loadable Modules)            │
│ ├── Process Management (Scheduler, CFS, Signals, Namespaces, Cgroups)   │
│ ├── Memory Management (Virtual Memory, MMU, Paging, Page Cache, OOM)    │
│ ├── Virtual Filesystem (VFS, Inodes, dentry cache, ext4/xfs/btrfs)      │
│ ├── Network Stack (Sockets, TCP/IP, Netfilter/iptables, eBPF)           │
│ └── Device Drivers (Block, Character, Network, PCI, NVMe, USB)          │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │ Interrupts / DMA / I/O Buses
┌────────────────────────────────────▼────────────────────────────────────┐
│ Layer 1: Physical Hardware & CPU Architecture                           │
│ (x86_64 / ARM64 CPU, MMU, Registers, RAM, NVMe/SSD, NIC, PCIe, GPU)   │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Module Learning Objectives

1. **Deconstruct the 5 Layers:** Understand the separation of concerns between user space applications and privileged kernel space.
2. **Master the System Call Boundary:** Learn how user applications transition from CPU Ring 3 (unprivileged) to Ring 0 (privileged) through the `syscall` instruction.
3. **Trace the End-to-End `cp` Workflow:** Follow a file copy (`cp source.txt dest.txt`) from shell input down to disk block I/O, page cache allocations, and storage interrupts.
4. **Inspect Live Kernel State:** Use `strace`, `ltrace`, `dmesg`, `uname`, `top`, and `/proc` to observe architecture in action.
5. **Architectural Trade-offs:** Contrast Monolithic kernels with Microkernels, and understand Loadable Kernel Modules (LKMs).
6. **SRE & DevOps Relevance:** Troubleshoot CPU steal time, context switching spikes, I/O wait deadlocks, and Out-Of-Memory (OOM) killer events.

---

## 📋 Module Contents

| File | Title | Key Topics Covered |
|---|---|---|
| [01-Architecture-Basics.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/01-Architecture-Basics.md) | Architecture Fundamentals | 5 layers, User space vs Kernel space, CPU privilege rings |
| [02-Kernel-Deep-Dive.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/02-Kernel-Deep-Dive.md) | The Linux Kernel Deep Dive | Monolithic vs Microkernel, LKMs, Scheduler, VFS, Memory |
| [03-System-Calls-and-Libraries.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/03-System-Calls-and-Libraries.md) | System Calls & glibc | C runtime library, syscall table, traps, return codes |
| [04-Utilities-and-User-Applications.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/04-Utilities-and-User-Applications.md) | Utilities & User Space | GNU coreutils, Shells, Daemons, systemd init system |
| [05-How-File-Copy-Works.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/05-How-File-Copy-Works.md) | End-to-End `cp` Workflow | Step-by-step trace: shell -> syscalls -> VFS -> drivers |
| [06-Practical-Commands.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/06-Practical-Commands.md) | Architecture Inspection | `strace`, `ltrace`, `lscpu`, `lsmod`, `modinfo`, `vmstat` |
| [07-Real-World-Scenarios.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/07-Real-World-Scenarios.md) | DevOps & Production Context | High context switching, OOM killer, eBPF, Container isolation |
| [08-Troubleshooting.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/08-Troubleshooting.md) | Diagnostic Playbooks | Tracing slow syscalls, kernel panics, high iowait, crashed daemons |
| [09-Interview-Q&A.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/09-Interview-Q&A.md) | Technical Interview QA | 10 in-depth architectural questions with answer frameworks |
| [10-Hands-On-Practice.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/10-Hands-On-Practice.md) | Terminal Exercises | Live labs with `strace`, kernel module inspection, and memory stats |
| [11-MCQ.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/11-MCQ.md) | Self-Assessment Quiz | 10 Multiple choice questions with thorough explanations |
| [12-Quick-Revision.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/12-Quick-Revision.md) | 5-Minute Summary | Quick cheat sheet, core concepts, syscall quick reference |
| [13-Related-Topics.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/13-Related-Topics.md) | Downstream Connections | Process lifecycle, Memory Paging, VFS internals, eBPF |
| [SOURCE.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen%27s/Linux/02-Linux-Architecture/SOURCE.md) | Source Material Mapping | Mapping of notes to the original Linux Architecture curriculum |
