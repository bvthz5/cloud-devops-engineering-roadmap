# 01 - Operating System Basics & The Core Abstraction

---

## 1. What is an Operating System?

An **Operating System (OS)** is system software that manages computer hardware and software resources, providing common services for computer programs. It acts as an essential intermediary between computer hardware and the human user or user-facing application programs.

Without an operating system, every software developer would have to write custom device drivers and machine code just to draw characters on a screen, spin up a storage disk, or read bytes from a network card.

---

## 2. The Core 4-Tier Abstraction Model

```
                    ┌─────────────────────────┐
                    │          User           │
                    │ (Human Engineer / Client│
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │       Application       │
                    │ (Nginx, Python, Docker) │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    Operating System     │
                    │   (Linux, Unix, BSD)    │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │        Hardware         │
                    │   (CPU, RAM, Disks, NIC)│
                    └─────────────────────────┘
```

1. **User:** Interacts with the computer to accomplish high-level business goals (browsing web pages, running automated pipelines, issuing shell commands).
2. **Application:** Software designed to perform specific computational tasks (compilers, databases, video encoders, microservices). Applications make requests to the operating system rather than communicating directly with silicon.
3. **Operating System:** Mediates, authorizes, schedules, and arbitrates access to the hardware components.
4. **Hardware:** The physical electronic components executing machine cycles and storing persistent charge (x86/ARM CPUs, volatile DRAM cells, flash NAND gates, network transceivers).

---

## 3. The Dual Role of an Operating System

Computer science identifies two fundamental views of an operating system:

```
                          ┌──────────────────────────┐
                          │     Operating System     │
                          └─────────────┬────────────┘
                                        │
           ┌────────────────────────────┴────────────────────────────┐
           ▼                                                         ▼
┌──────────────────────────────────────┐  ┌──────────────────────────────────────┐
│       1. The Abstract Machine        │  │       2. The Resource Manager        │
│       (Top-Down Perspective)         │  │       (Bottom-Up Perspective)        │
├──────────────────────────────────────┤  ├──────────────────────────────────────┤
│ • Transforms complex, messy hardware │  │ • Manages competing requests for CPU │
│   into clean, elegant abstractions.  │    time, RAM blocks, and network I/O.   │
│ • Examples:                          │  │ • Prevents starvation, enforces fair │
│   - Turns disk sectors into "Files"  │    scheduling, isolates faulty tasks.   │
│   - Turns CPU execution into "Tasks" │  │ • Enforces security, permissions,    │
│   - Turns memory addresses into "VM" │    and process boundaries.              │
└──────────────────────────────────────┘  └──────────────────────────────────────┘
```

### The Top-Down View: Extended / Abstract Machine
Raw hardware is difficult to program directly. Disk drives require sending specific SCSI or NVMe commands to write blocks, verify checksums, and manage bad sectors. The OS provides the **File** abstraction—a named sequence of bytes that can be opened, read, written, and closed seamlessly.

### The Bottom-Up View: Resource Manager
When multiple applications run simultaneously (e.g., a database, an HTTP server, and a backup script), they compete for finite physical components. The OS acts as a resource arbiter:
- **Time Multiplexing:** Sharing a resource by allocating time slices (e.g., CPU scheduling: Process A runs for 10ms, then Process B runs for 10ms).
- **Space Multiplexing:** Sharing a resource by partitioning capacity (e.g., dividing physical RAM among different running processes).

---

## 4. Primary Goals of an Operating System
1. **Convenience:** Makes the computer system convenient and intuitive to operate.
2. **Efficiency:** Maximizes throughput and minimizes latency across physical CPU cores and I/O channels.
3. **Reliability & Isolation:** Ensures that a bug or crash in one user space program cannot crash neighboring applications or take down the host.
4. **Extensibility & Portability:** Provides standardized APIs (such as POSIX) allowing software to run across diverse hardware architectures without modification.
