# 03 — Process Lifecycle, fork/exec, Process States, Zombies, and Orphans

---

## 1. What Is a Process?

A **Process** is an active instance of a computer program in execution. Unlike a passive executable file resting on disk (such as `/usr/bin/nginx`), a process is a dynamic entity possessing allocated system resources:
- Dedicated **Virtual Address Space** (code, data, heap, and stack).
- **Process ID (PID):** A unique numerical identifier assigned by the kernel.
- **Process Control Block (PCB):** Represented in the Linux kernel by the `struct task_struct`.
- **File Descriptor Table:** Pointers to open files, pipes, and network sockets.
- **Security Context:** Real User ID (UID), Effective User ID (EUID), Group IDs, and Linux Capabilities.
- **CPU Context:** Saved hardware registers, Program Counter (`PC`), and Stack Pointer (`SP`).

---

## 2. Linux Process Hierarchy and the Process Tree

In Linux, all processes form a strict hierarchical tree originating from a single common ancestor: **PID 1 (`systemd` on modern Linux, or `/sbin/init`)**.

```text
               systemd (PID 1)
                ├── systemd-journald (PID 412)
                ├── sshd (PID 890)
                │    └── sshd: ubuntu [priv] (PID 2145)
                │         └── bash (PID 2150)
                │              └── python3 app.py (PID 3400)
                └── containerd (PID 1102)
                     └── containerd-shim (PID 4010)
                          └── node server.js (PID 4025)
```

- **PID (Process ID):** Unique identifier (range `1` to `32768` by default, configurable via `/proc/sys/kernel/pid_max`).
- **PPID (Parent Process ID):** The PID of the process that spawned this process.
- **Process Tree Inspection:**
  ```bash
  # View full process hierarchy tree
  pstree -p
  
  # Inspect specific process lineage
  ps -o pid,ppid,user,stat,comm -p 3400
  ```

---

## 3. Process Lifecycle & State Machine

During its existence, a process transitions between distinct kernel states:

```text
                  +-------------+
                  |  Created    |
                  | (fork/clone)|
                  +------+------+
                         │
                         ▼
        +────────► [ TASK_RUNNING ] ◄────────┐
        │            (State: R)              │
        │          Running on CPU            │
        │          or Ready to Run           │
        │                │                   │
CPU Scheduler     Waiting for I/O     I/O Operation
Time Slice Over   or Event            Completes
        │                │                   │
        │                ▼                   │
        │       [ TASK_INTERRUPTIBLE ]       │
        │            (State: S)              │
        │      Sleeping (Interruptible)      │
        │                │                   │
        │                ▼                   │
        │     [ TASK_UNINTERRUPTIBLE ] ──────┘
        │            (State: D)
        │     Disk I/O Wait (Unkillable)
        │
   SIGSTOP / Ctrl+Z
        │
        ▼
   [ TASK_STOPPED ] (State: T)
        │
   Process Terminated (exit)
        │
        ▼
   [ EXIT_ZOMBIE ] (State: Z)
   (Defunct: Waiting for parent wait() call)
        │
   Parent calls wait()
        │
        ▼
     [ DEAD ]
   (PCB and PID freed from kernel memory)
```

### Detailed Process State Breakdown

| State Code | Kernel Name | Description | SRE / DevOps Significance |
| :--- | :--- | :--- | :--- |
| **`R`** | `TASK_RUNNING` | Process is either currently executing on a CPU core or waiting in the scheduler's run queue. | High `R` count indicates CPU saturation or runaway loop. |
| **`S`** | `TASK_INTERRUPTIBLE` | Process is sleeping, waiting for an event (socket packet, timer, or user input). It will wake up if sent a signal. | Normal state for idle microservices, web servers, or waiting threads. |
| **`D`** | `TASK_UNINTERRUPTIBLE` | Process is waiting on a hardware event (usually direct disk I/O, NFS lock, or paging). **It cannot be killed even by `SIGKILL` (`kill -9`)!** | A build-up of `D` state processes indicates failing storage, frozen NFS mounts, or kernel deadlocks. |
| **`T`** | `TASK_STOPPED` | Process was suspended by a signal (e.g., `SIGSTOP` or interactive `Ctrl+Z`). | Resumed using `kill -CONT <PID>` or shell `fg`. |
| **`Z`** | `EXIT_ZOMBIE` | Process has finished execution, but its exit status has not yet been read by its parent. | Consumes no CPU or RAM, but consumes a PID slot. |

---

## 4. How Processes Are Created: `fork()` and `exec()`

Linux does not create a new process from scratch in a single step. Instead, process creation is decoupled into two distinct system calls: **`fork()`** and **`execve()`**.

```text
Parent Process (PID 1000)
    │
    ├─► 1. Calls fork()
    │      Kernel duplicates Parent's memory (via Copy-On-Write)
    │      Creates Child Process (PID 1001)
    │
    │   Child Process (PID 1001)
    │   │
    │   ├─► 2. Calls execve("/bin/ls", ["ls", "-l"], NULL)
    │   │      Kernel wipes child's address space
    │   │      Loads "/bin/ls" executable from disk
    │   │      Starts executing main() of "ls"
    │   │
    │   └─► 3. Calls exit(0)
    │          Child terminates with status code 0
    │
    └─► 4. Parent calls waitpid(1001)
           Reads exit code 0
           Kernel frees Child PID 1001
```

### 1. `fork()`
- Creates an exact duplicate of the calling process.
- **Copy-On-Write (COW):** The kernel does not copy physical RAM pages during `fork()`. Instead, both parent and child page tables point to the **same physical frames**, marked as **read-only**. Only when either process writes to a page does the MMU trigger a page fault, prompting the kernel to allocate and copy that single 4 KB page. This makes `fork()` instantaneous.
- **Return Values:**
  - In the **Parent process:** `fork()` returns the newly created child's PID.
  - In the **Child process:** `fork()` returns `0`.
  - On **Failure:** Returns `-1`.

### 2. `execve()`
- Replaces the current process image with an entirely new program binary loaded from disk.
- Retains the same PID, PPID, and open file descriptors (unless marked `FD_CLOEXEC`).
- Re-initializes the stack, heap, and BSS data segments.

### 3. `wait()` / `waitpid()`
- Suspends the parent process until one of its children terminates.
- Retrieves the termination status (exit code, or signal that killed the child).
- Signals the kernel that the child's entry in the Process Table can now be safely erased.

---

## 5. Zombie Processes (`defunct`)

### What Is a Zombie Process?
When a process executes `exit()`, the kernel releases its physical memory, heap, and open file descriptors. However, the kernel **must retain the process's entry in the process table** (`task_struct`), keeping its PID and exit status code alive until the parent process calls `wait()`.

During this intermediate phase, the dead process is called a **Zombie** (marked as `<defunct>` or state `Z` in `ps`).

```text
Parent Process (PID 500) ───[ Runs application logic, ignores child ]───
                                        ▲
                                        │ (Exit signal sent, but no wait())
Child Process (PID 501) ────[ exit(0) ] ┼──► [ ZOMBIE STATE ]
                                             Consumes PID slot
                                             Holds task_struct in kernel
```

### Why Doesn't `kill -9 <zombie_pid>` Work?
> **CRITICAL PRODUCTION FACT:** A zombie is already dead! You cannot kill what is already dead. `SIGKILL` cannot be delivered to a process with no user space execution thread.

### How to Clean Up Zombie Processes
1. **Signal the Parent:** Send `SIGCHLD` to the parent process to trigger its signal handler:
   ```bash
   kill -s SIGCHLD <parent_pid>
   ```
2. **Kill the Parent:** If the parent is broken or hung and refuses to reap its child, kill the **parent process**:
   ```bash
   kill -9 <parent_pid>
   ```
   When the parent terminates, the zombie becomes an **Orphan** and is adopted by **PID 1 (`systemd`)**. PID 1 continuously calls `wait()`, instantly reaping and eliminating the zombie.

---

## 6. Orphan Processes

An **Orphan Process** is an active, executing process whose parent terminated or crashed before the child finished.

### What Happens to Orphans?
In Linux, a child process cannot exist without a parent. When a parent dies:
1. The kernel immediately re-parents all orphaned children to **PID 1 (`systemd`)** or an intermediate subreaper process (e.g., `dumb-init` or `tini` inside a Docker container).
2. When the orphan eventually terminates, PID 1 reaps its exit code, preventing it from ever becoming a persistent zombie.

---

## 7. Container Implication: PID 1 Problem in Docker

Inside a Docker container without an init system:
- If your container entrypoint is `node server.js` or `python app.py`, that application runs as **PID 1**.
- Most language runtimes **do not implement a `SIGCHLD` reaper** or forward signals properly!
- Result: Subprocesses spawned by the container accumulate as zombies until the kernel runs out of PIDs (`pid_max`), causing the host to crash.
- **Remedy:** Always run containers with an init wrapper:
  ```bash
  docker run --init my-container-image
  ```
  Or use `tini` / `dumb-init` in your `Dockerfile`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - System Calls Interface and Categories](./02-System-Calls-Interface-and-Categories.md) | [Index](../../../README.md) | [04 - Threads Multithreading and CPU Scheduling →](./04-Threads-Multithreading-and-CPU-Scheduling.md) |
