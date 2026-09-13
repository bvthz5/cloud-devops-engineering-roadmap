# 04 - System Calls: The Controlled Gateway

A **system call (syscall)** is the fundamental programmatic gateway that allows an unprivileged user space application to request services from the privileged Linux kernel.

---

## 1. The Syscall Gateway Mechanism

User applications cannot execute privileged kernel code directly. Instead, they must issue a software trap that transfers execution to a strictly verified kernel entry point:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. User Application (Python / Go / C / Nginx)              │
│    Calls standard POSIX library function: write(fd, buf, n) │
└──────────────────────────────┬──────────────────────────────┘
                               │
┌──────────────────────────────▼──────────────────────────────┐
│ 2. C Standard Library (glibc / musl)                        │
│    • Moves argument values into CPU registers               │
│    • Loads the write() syscall number (1) into %rax         │
│    • Executes CPU instruction: `syscall`                    │
└──────────────────────────────┬──────────────────────────────┘
                               │ CPU switches from Ring 3 to Ring 0
═══════════════════════════════╪═══════════════════════════════
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. Linux Kernel Syscall Entry Handler                       │
│    • Saves user registers to kernel stack                   │
│    • Validates %rax against sys_call_table bounds           │
│    • Invokes: ksys_write(fd, buf, count)                    │
│    • Stores return value in %rax                            │
│    • Executes `sysret` to restore Ring 3 and resume app     │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Essential System Calls Grouped by Subsystem

The Linux kernel exposes approximately 450 system calls. The most critical for systems engineering are:

### 1. Process & Thread Control:
- **`clone()` / `fork()`:** Creates a new child process or lightweight thread (Docker uses `clone()` with flags like `CLONE_NEWPID` to create containers!).
- **`execve()`:** Replaces the current process memory image with a new executable program.
- **`exit()` / `exit_group()`:** Terminates execution of a thread or process.
- **`wait4()` / `waitpid()`:** Suspends the parent process until a child process terminates, reading its exit status.

### 2. Filesystem & Storage Operations:
- **`openat()`:** Opens a file relative to a directory file descriptor and returns a new integer **File Descriptor (FD)**.
- **`read()`:** Reads raw bytes from an open file descriptor into a user memory buffer.
- **`write()`:** Writes raw bytes from a user memory buffer to an open file descriptor.
- **`close()`:** Releases the process's reference to the open file descriptor.
- **`copy_file_range()`:** Performs high-speed in-kernel zero-copy file duplication.

### 3. Memory Management:
- **`mmap()`:** Maps files or anonymous physical memory pages directly into the process's virtual address space (used by databases and `malloc`).
- **`brk()`:** Adjusts the end address of the process data segment to allocate or free heap memory.
- **`mprotect()`:** Changes memory protection permissions (`PROT_READ`, `PROT_WRITE`, `PROT_EXEC`) on memory regions.

### 4. Networking & IPC:
- **`socket()`:** Creates an endpoint for network communication.
- **`bind()` & `listen()`:** Binds a socket to an IP/port and marks it as passive, ready to accept connections.
- **`accept()` / `accept4()`:** Extracts the first incoming connection request on a listening socket.
- **`epoll_create()` / `epoll_wait()`:** Scalable I/O event notification mechanism powering high-performance servers like Nginx and Node.js.
