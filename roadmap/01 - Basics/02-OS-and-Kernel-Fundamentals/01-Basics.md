# Operating System and Kernel Fundamentals — Core Concepts

## 1. What Is an Operating System & Kernel?

An Operating System (OS) is the master control software managing computer hardware and presenting standardized system services to applications.

The **Kernel** is the core, privileged component of the OS running directly on hardware.

```text
User / Developer
       ↓
  Applications (Web Server, Database, Browser)
       ↓  System Calls
Operating System Kernel (Monolithic / Hybrid)
       ↓
  Hardware (CPU, RAM, Disks, NIC)
```

---

## 2. User Space vs. Kernel Space (Protection Rings)

Modern CPUs implement privilege rings to prevent user applications from corrupting hardware or system memory.

```text
     ┌─────────────────────────────────────────────────────────────┐
     │  Ring 3: User Space                                         │
     │  - Applications (Nginx, Python, Go, Node.js)               │
     │  - Shells (Bash, Zsh) & User Libraries                      │
     └──────────────────────────────┬──────────────────────────────┘
                                    │ System Call (e.g. syscall / int 0x80)
     ┌──────────────────────────────▼──────────────────────────────┐
     │  Ring 0: Kernel Space (Privileged)                          │
     │  - Process Scheduler, Memory Manager, Filesystems           │
     │  - Network Stack, Device Drivers, Security Controls         │
     └─────────────────────────────────────────────────────────────┘
```

---

## 3. System Calls (Syscalls)

System calls are controlled gateways allowing unprivileged user programs to request kernel operations.

### Essential POSIX System Calls

| System Call | Purpose |
|---|---|
| `fork()` | Creates a duplicate child process. |
| `execve()` | Replaces the current process image with a new program executable. |
| `open() / close()` | Opens or closes a file, returning a File Descriptor. |
| `read() / write()` | Reads or writes data bytes from/to a File Descriptor. |
| `socket() / connect()` | Creates a network endpoint and establishes a TCP connection. |
| `wait() / waitpid()` | Blocks parent process until a child process changes state or exits. |

---

## 4. Processes, PIDs & Process States

- **Process:** An executing instance of a program holding its own virtual address space, file descriptor table, and credentials.
- **Process ID (PID):** Unique integer identifying a process.
- **PID 1 (`init` / `systemd`):** The first user space process spawned by kernel boot. Adopted orphans and manages system daemons.

### Process State Lifecycle

```text
               ┌──────────┐
               │ Created  │
               └────┬─────┘
                    │
                    ▼
 ┌──────────┐  Scheduled  ┌─────────┐
 │ Runnable │ ──────────> │ Running │
 └────▲─────┘             └────┬────┘
      │                        │ Waiting for I/O / Signal
      │     I/O Complete       ▼
      └────────────────── ┌─────────┐
                          │ Sleeping│
                          └─────────┘
```

- **Zombie Process (`Z`):** A terminated process whose parent has not yet called `wait()` to read its exit status code.
- **Orphan Process:** A process whose parent died before it. Immediately adopted by PID 1.

---

## 5. Memory Management & OOM Killer

- **Virtual Memory:** Provides each process with a contiguous 64-bit address space, mapped to non-contiguous physical RAM pages.
- **Page Fault:** Triggered when a process accesses a page not currently resident in RAM (requiring swap retrieval or copy-on-write allocation).
- **Out-Of-Memory (OOM) Killer:** Kernel subsystem that monitors memory pressure. When physical RAM and swap are exhausted, it calculates `oom_score` for all processes and sends `SIGKILL` (9) to the highest-scoring process to preserve system stability.

---

## 6. File Descriptors & Standard Streams

In Unix systems, *"everything is a file"*. All open I/O resources (files, sockets, pipes, devices) are referenced by integer File Descriptors (FDs).

### Standard File Descriptors

```text
FD 0 → stdin  (Standard Input, default: keyboard)
FD 1 → stdout (Standard Output, default: screen)
FD 2 → stderr (Standard Error, default: screen)
```

---

## 7. Environment Variables & Daemons

- **Environment Variables:** Key-value pairs stored in process memory passed to child processes during `fork/execve`. Crucial for microservice configuration (`PORT`, `DB_HOST`).
- **Daemon / Service:** Long-running background process without a controlling terminal (e.g., `sshd`, `dockerd`, `systemd`).
- **Kernel Security Boundaries:** Kernel primitives like **Linux Namespaces** (mount, PID, network, IPC) and **Control Groups (cgroups)** form the architectural engine behind containers!
