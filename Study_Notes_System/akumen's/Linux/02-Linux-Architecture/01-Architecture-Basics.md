# 01 - Architecture Basics & The 5-Layer Model

---

## 1. What is the Purpose of an Operating System?

The primary job of the Linux operating system is to act as an intermediary between physical hardware components and user-facing applications. It provides two foundational services:
1. **Resource Abstraction:** Hiding raw hardware quirks (registers, clock pulses, storage sectors) behind clean, unified APIs (`read()`, `write()`, `socket()`).
2. **Resource Management:** Arbitrating fair and secure access to CPU cores, RAM, network bandwidth, and persistent storage among hundreds of competing processes.

---

## 2. The 5 Core Architectural Layers

```
Layer 5: User Applications
    ├── Web browsers, microservices, databases, Python runtimes
    └── Execute in unprivileged User Space (Ring 3)
         │
Layer 4: System Utilities & Shells
    ├── bash, zsh, coreutils (ls, cp, grep), systemd, sshd
    └── Provide command-line control and service supervision
         │
Layer 3: System Libraries (glibc, musl)
    ├── Standard C runtime library implementing the POSIX API
    └── Wraps raw system call numbers into convenient functions
         │
════════════ System Call Boundary (Trap / Syscall Instruction) ════════════
         │
Layer 2: The Linux Kernel
    ├── Monolithic core managing processes, memory, files, and drivers
    └── Executes in privileged Kernel Space (Ring 0)
         │
Layer 1: Physical Hardware
    └── CPU, MMU, Physical RAM, NVMe/SATA Disks, Network Interface Cards (NIC)
```

### Detailed Breakdown of Each Layer:

| Layer | Name | Description | Key Components |
|---|---|---|---|
| **Layer 5** | **User Applications** | End-user programs, cloud services, and developer tools. | Nginx, Docker, Node.js, Python, PostgreSQL |
| **Layer 4** | **System Utilities** | Management commands, administrative daemons, and shells. | `bash`, `systemd`, `sshd`, `cp`, `cat`, `ps` |
| **Layer 3** | **System Libraries** | Bridges userland code to kernel APIs via standardized C runtime functions. | `glibc` (`libc.so.6`), `musl-libc`, `libpthread` |
| **Layer 2** | **Linux Kernel** | The core supervisor controlling CPU scheduling, memory, VFS, and drivers. | CFS Scheduler, Page Cache, VFS, Netfilter, Device Drivers |
| **Layer 1** | **Hardware** | The physical silicon and peripheral controllers. | x86_64/ARM64 CPU, RAM, MMU, Storage Disks, NICs |

---

## 3. Kernel Space vs. User Space

To prevent a buggy program or malicious script from crashing the entire server or stealing data directly from RAM, modern CPU architectures enforce a strict boundary between **User Space** and **Kernel Space**.

```
┌────────────────────────────────────────────────────────┐
│                      USER SPACE                        │
│ • Unprivileged mode (CPU Ring 3)                       │
│ • Restricted memory access (isolated virtual memory)   │
│ • Cannot directly execute I/O instructions (in, out)   │
│ • If a process crashes (Segfault), ONLY it dies        │
└───────────────────────────┬────────────────────────────┘
                            │ System Call (`syscall` instruction)
                            ▼ (Switches CPU Mode to Ring 0)
┌────────────────────────────────────────────────────────┐
│                     KERNEL SPACE                       │
│ • Privileged mode (CPU Ring 0)                         │
│ • Full, direct access to physical memory & hardware    │
│ • Executes device drivers and hardware interrupt code  │
│ • If the kernel crashes -> KERNEL PANIC (system halts) │
└────────────────────────────────────────────────────────┘
```

---

## 4. Hardware CPU Rings (x86 Architecture)

The x86 CPU architecture provides four hierarchical privilege rings (Rings 0 to 3). Linux utilizes two of them:

```
          ┌───────────────────────────────────┐
          │ Ring 3: User Space Applications   │  (Least Privileged)
          │  ┌─────────────────────────────┐  │
          │  │ Ring 2: [Unused in Linux]   │  │
          │  │  ┌───────────────────────┐  │  │
          │  │  │ Ring 1: [Unused]      │  │  │
          │  │  │  ┌─────────────────┐  │  │  │
          │  │  │  │ Ring 0: Kernel  │  │  │  │  (Most Privileged)
          │  │  │  └─────────────────┘  │  │  │
          │  │  └───────────────────────┘  │  │
          │  └─────────────────────────────┘  │
          └───────────────────────────────────┘
```

- **Ring 0 (Supervisor Mode):** The CPU allows execution of all instructions, including memory management table modifications (`CR3` register), interrupt handling, and hardware port I/O. The Linux kernel executes entirely in Ring 0.
- **Ring 3 (User Mode):** Normal applications execute here. Direct memory access to other processes or hardware registers is forbidden by the CPU's Memory Management Unit (MMU). Any unauthorized memory access triggers a CPU Exception (`General Protection Fault` or `Page Fault`), which the kernel translates into a `SIGSEGV` (Segmentation Fault) sent to the offending process.

---

## 5. Mode Switch vs. Context Switch

A common point of confusion in system performance is the difference between a **Mode Switch** and a **Process Context Switch**:

### 1. Mode Switch (Ring 3 ➔ Ring 0)
- Occurs when a user process invokes a system call (e.g., calling `read()`).
- The CPU switches privilege levels from Ring 3 to Ring 0.
- **The process stays the same!** The kernel executes the system call on behalf of the same calling process using that process's dedicated kernel stack.
- Highly optimized and relatively fast (typically ~50 to 100 nanoseconds).

### 2. Process Context Switch (Process A ➔ Process B)
- Occurs when the Linux CPU scheduler suspends Process A and starts executing Process B.
- Requires saving Process A's CPU registers, switching page tables (`CR3` register), and invalidating/flushing the CPU Translation Lookaside Buffer (TLB).
- Significantly more expensive than a mode switch. High context switching rates directly degrade system throughput.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Kernel Deep Dive](./02-Kernel-Deep-Dive.md) |
