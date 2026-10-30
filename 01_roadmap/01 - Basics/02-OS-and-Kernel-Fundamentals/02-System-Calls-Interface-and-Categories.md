# 02 — System Calls, Interfaces, and Categories

---

## 1. What Is a System Call (Syscall)?

A **System Call** is the programmatic mechanism by which an unprivileged user space program requests a privileged service from the operating system kernel.

Because user space processes run in **Ring 3** and cannot directly execute I/O instructions, manipulate hardware registers, or touch raw disks, they must ask the kernel (running in **Ring 0**) to perform these operations on their behalf.

```text
User Application (Python / Go / C)
       │
       ▼ Calls C Library Wrapper (glibc)
       │  e.g., read(fd, buf, count)
       ▼
Sets CPU Registers (RAX=0, RDI=fd, RSI=buf, RDX=count)
       │
       ▼ Executes 'syscall' Instruction
===================================================== [Ring 3 -> Ring 0 Trap]
KERNEL SPACE:
  1. Validates user pointers and memory bounds
  2. Looks up Syscall #0 in sys_call_table
  3. Executes sys_read()
  4. Places return value in RAX
  5. Executes 'sysret'
===================================================== [Ring 0 -> Ring 3 Return]
User Application receives result bytes
```

---

## 2. The System Call Interface (SCI)

The **System Call Interface** consists of the assembly-level dispatch table and standard C library (`glibc` or `musl`) wrapper functions that bridge high-level programming languages with kernel internals.

### System Call Architecture on x86_64 Linux

| Stage | Mechanism / Register | Description |
| :--- | :--- | :--- |
| **Syscall Number** | `RAX` Register | Each kernel function has a unique ID (e.g., `0 = read`, `1 = write`, `2 = open`, `57 = fork`, `60 = exit`). |
| **1st Argument** | `RDI` Register | First parameter (e.g., file descriptor or pointer). |
| **2nd Argument** | `RSI` Register | Second parameter (e.g., buffer pointer). |
| **3rd Argument** | `RDX` Register | Third parameter (e.g., byte length). |
| **4th – 6th Arguments**| `R10`, `R8`, `R9` | Additional parameters (up to 6 registers in Linux ABI). |
| **Trigger Instruction** | `syscall` | Triggers hardware trap to kernel mode (`sysenter` on older 32-bit x86). |
| **Return Code** | `RAX` Register | Contains return value (bytes read, or negative `-errno` on error). |

---

## 3. The 5 Major Categories of System Calls

Linux implements roughly 450 system calls, categorized into five core functional groups:

```text
+-------------------------------------------------------------+
|                    System Call Categories                   |
+-------------------------------------------------------------+
| 1. Process Control     | fork, clone, execve, exit, wait4   |
| 2. File Management     | open, read, write, close, unlink   |
| 3. Device Management   | ioctl, read, write                 |
| 4. Information Maint.  | getpid, time, gettimeofday, uname  |
| 5. Communication / IPC | socket, connect, bind, pipe, futex |
+-------------------------------------------------------------+
```

### 1. Process Control Syscalls
These calls govern how processes are created, executed, synchronized, and terminated:
- **`fork()` / `clone()`:** Creates a new child process duplicating the calling process. `clone()` is the underlying powerhouse used by Docker and threads.
- **`execve()`:** Replaces the current process memory image with an entirely new binary executable.
- **`exit()` / `exit_group()`:** Terminates process execution and yields exit status to the kernel.
- **`wait4()` / `waitpid()`:** Suspends the calling parent process until a child process changes state or exits.

### 2. File Management Syscalls
These calls handle the creation, manipulation, and deletion of files and directories:
- **`openat()` / `open()`:** Opens or creates a file, returning a non-negative integer file descriptor.
- **`read()`:** Reads raw bytes from an open file descriptor into a user space memory buffer.
- **`write()`:** Writes bytes from a user space memory buffer to an open file descriptor.
- **`close()`:** Releases the file descriptor back to the kernel's file table.
- **`stat()` / `fstatat()`:** Retrieves file metadata (size, permissions, timestamps, inode number).
- **`unlink()`:** Removes a directory entry; if it was the last hard link, frees the inode.

### 3. Device Management Syscalls
- **`ioctl()` (I/O Control):** Swiss-army-knife syscall for device-specific operations that do not map to standard read/write streams (e.g., querying NIC link speed, setting terminal window size).
- **`read()` / `write()`:** Because Linux adheres to the "Everything is a file" philosophy, character devices (like `/dev/tty`) and block devices (like `/dev/sda`) use standard file read/write calls.

### 4. Information Maintenance Syscalls
- **`getpid()`:** Returns the caller's unique Process ID.
- **`getppid()`:** Returns the parent Process ID.
- **`uname()`:** Returns OS name, kernel version, hostname, and hardware architecture.
- **`sysinfo()`:** Returns system uptime, memory load, and swap statistics.

### 5. Communication & IPC Syscalls
- **`pipe()` / `pipe2()`:** Creates a unidirectional inter-process data channel.
- **`socket()`:** Creates an endpoint for communication (TCP/UDP or local Unix Domain Socket).
- **`bind()` / `listen()` / `accept()`:** Server-side socket workflow.
- **`connect()`:** Client-side socket connection initiation.
- **`futex()` (Fast Userspace Mutex):** Low-level synchronization primitive enabling user space locks without kernel context switches unless contention occurs.

---

## 4. Observing Syscalls in Action with `strace`

In DevOps and SRE, **`strace`** is the premier diagnostic tool to trace system calls made by any running process. It intercepts and logs every system call made and received by a program.

### Example: Tracing `echo "Hello World"`
```bash
strace -c echo "Hello World"
```

Output Summary Table:
```text
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 35.12    0.000045           7         6           mmap
 22.18    0.000028           7         4           mprotect
 18.05    0.000023          23         1           write
 10.45    0.000013           4         3           openat
  8.20    0.000010           3         3           close
  6.00    0.000008           2         4           read
------ ----------- ----------- --------- --------- ----------------
100.00    0.000127                    21           total
```

### Inspecting a Live Production Service
To see what a stuck or hanging process is doing right now:
```bash
# Attach to running process with PID 1845
sudo strace -p 1845 -f -s 128 -T
```
- `-p 1845`: Attach to PID.
- `-f`: Follow child threads/processes.
- `-s 128`: Print up to 128 bytes of string arguments.
- `-T`: Print time spent inside each system call.

---

## 5. System Call Overhead & Performance Implications

Making a system call is significantly more expensive than an in-memory function call:
1. **CPU Context Switch Overhead:** Saving user space registers, switching hardware privilege rings (Ring 3 → Ring 0), swapping page table roots or updating TLB structures.
2. **Cache Pollution:** The CPU instruction cache must load kernel code, evicting warm application instructions.
3. **Branch Prediction Penalty:** Meltdown/Spectre security mitigations (KPTI - Kernel Page Table Isolation) add memory barrier instructions on every kernel entry/exit.

> **Cloud Performance Rule:** High-performance systems (like Nginx, Redis, and Envoy) minimize syscall overhead by using **batching** (e.g., `readv`, `writev`), **memory-mapped files** (`mmap`), and modern asynchronous ring buffers (**`io_uring`**).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - OS Architecture Kernel Types and Dual Mode](./01-OS-Architecture-Kernel-Types-and-Dual-Mode.md) | [Index](../../../README.md) | [03 - Process Lifecycle fork exec States and Zombies →](./03-Process-Lifecycle-fork-exec-States-and-Zombies.md) |
