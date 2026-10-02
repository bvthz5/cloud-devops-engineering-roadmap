# 07 — Signals and Inter-Process Communication (IPC)

---

## 1. Operating System Signals

A **Signal** is an asynchronous notification sent by the Linux kernel to a process to notify it that an event has occurred. Signals function as software interrupts: when a signal is delivered, the target process halts its normal execution thread and jumps immediately to a registered **Signal Handler** routine.

```text
Kernel / External Process (kill -15)
              │
              ▼ Delivers Signal (e.g., SIGTERM)
   +─────────────────────────────────────+
   | Target Process Execution            |
   |                                     |
   | [ Normal Code Execution ]           |
   |          │                          |
   |          ▼ Interrupted              |
   | [ Execute Signal Handler Function ] |
   |   - Clean up DB connections         |
   |   - Flush logs to disk              |
   |   - Call exit(0)                    |
   +─────────────────────────────────────+
```

---

## 2. Essential Linux Signals for DevOps & SREs

Linux implements 64 signals (standard signals 1–31 and real-time signals 34–64). Below are the most critical signals encountered in production:

| Signal | Number | Default Action | Catchable? | Purpose & Cloud Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **`SIGHUP`** | `1` | Terminate | Yes | **Hangup / Configuration Reload.** Sent when terminal closes, or used to trigger zero-downtime config reload in Nginx/Prometheus (`kill -HUP <PID>`). |
| **`SIGINT`** | `2` | Terminate | Yes | **Interactive Interrupt.** Sent when user presses `Ctrl+C` in terminal. |
| **`SIGQUIT`**| `3` | Core Dump | Yes | **Quit.** Generates a memory core dump file on termination (`Ctrl+\`). |
| **`SIGKILL`**| `9` | Terminate | **NO** | **Forced Immediate Kill.** Kernel immediately terminates the process and reclaims memory. Process cannot catch, block, or clean up resources! |
| **`SIGSEGV`**| `11`| Core Dump | Yes | **Segmentation Violation.** Memory access violation (reading invalid pointer or writing to read-only page). |
| **`SIGPIPE`**| `13`| Terminate | Yes | **Broken Pipe.** Process wrote to a pipe/socket whose reading end was closed. |
| **`SIGTERM`**| `15`| Terminate | Yes | **Graceful Termination.** Default signal sent by `kill <PID>`, `systemctl stop`, and **Kubernetes during Pod eviction**. |
| **`SIGUSR1`**| `10`| Terminate | Yes | User-defined signal 1. (e.g., Nginx reopens log files for rotation). |
| **`SIGUSR2`**| `12`| Terminate | Yes | User-defined signal 2. (e.g., Nginx zero-downtime binary upgrade). |
| **`SIGCHLD`**| `17`| Ignore | Yes | Sent to parent whenever a child process terminates, stops, or resumes. Used for reaping zombies. |
| **`SIGSTOP`**| `19`| Pause | **NO** | **Uncatchable Pause.** Freezes process execution immediately (`Ctrl+Z`). Resumed with `SIGCONT`. |

> **Kubernetes Pod Termination Lifecycle:**
> 1. Pod marked `Terminating`, removed from Service endpoints.
> 2. Kubelet sends **`SIGTERM` (Signal 15)** to container PID 1.
> 3. Kubelet waits for **`terminationGracePeriodSeconds`** (default 30 seconds) allowing in-flight requests to complete.
> 4. If the process has not exited after 30 seconds, Kubelet sends **`SIGKILL` (Signal 9)** to forcibly terminate it.

---

## 3. Inter-Process Communication (IPC) Mechanisms

Processes possess isolated address spaces. When two processes need to exchange data or synchronize actions, they must utilize kernel-mediated **Inter-Process Communication (IPC)**.

```text
+-------------------------------------------------------------+
|               Linux IPC Mechanisms Spectrum                 |
+-------------------------------------------------------------+
| Mechanism           | Speed / Latency | Persistence | Scope  |
+---------------------+-----------------+-------------+-------+
| Anonymous Pipes     | High (~1-2 µs)  | Process-life| Local |
| Named Pipes (FIFOs) | High (~1-2 µs)  | Filesystem  | Local |
| Unix Domain Sockets | Very High (~1µs)| Filesystem  | Local |
| Shared Memory       | Ultra-Fast(0ns) | Kernel-life | Local |
| Network Sockets     | Moderate(~50µs) | Network     | Remote|
| Message Queues      | High (~2-5 µs)  | Kernel-life | Local |
| Semaphores          | Fast (Sync)     | Kernel-life | Local |
+-------------------------------------------------------------+
```

---

## 4. Deep Dive into IPC Mechanisms

### 1. Anonymous Pipes (`|`)
- Unidirectional data channels created by the `pipe()` system call.
- Allocated as an in-kernel circular ring buffer (typically 64 KB).
- Connects standard output (`stdout`) of one process to standard input (`stdin`) of another:
  ```bash
  cat /var/log/nginx/access.log | grep "500" | wc -l
  ```

### 2. Named Pipes (FIFOs)
- Behaves like a pipe, but exists as a concrete file node on the filesystem (`mkfifo`).
- Unrelated processes can open it by filename:
  ```bash
  # Terminal 1:
  mkfifo /tmp/log_stream
  cat < /tmp/log_stream
  
  # Terminal 2:
  echo "Critical DB Error" > /tmp/log_stream
  ```

### 3. Unix Domain Sockets (UDS)
- Bidirectional communication endpoint exposed as a filesystem socket node (`.sock`).
- Standard IPC protocol for local services (e.g., Docker daemon `/var/run/docker.sock`, containerd `/run/containerd/containerd.sock`, PostgreSQL local connections).
- **Why UDS beats Localhost TCP (`127.0.0.1`):**
  - Completely bypasses TCP/IP network stack (no IP routing, checksums, TCP 3-way handshake, or packet fragmentation).
  - Can pass **open File Descriptors** between processes using `SCM_RIGHTS`.
  - Enforces standard file permission checks (UID/GID) for client authorization.

### 4. Shared Memory (`/dev/shm` and POSIX `shm_open`)
- The **fastest IPC mechanism possible**.
- Two or more processes map the exact same physical DRAM pages into their respective virtual address spaces.
- Data written by Process A is instantly readable by Process B with **zero kernel mediation and zero memory copies**!
- Because access is simultaneous, processes must use **Semaphores** or **Mutexes** to prevent data corruption.
- In Linux, available as an in-memory `tmpfs` at `/dev/shm`:
  ```bash
  df -h /dev/shm
  ```

### 5. Semaphores & Mutexes
- **Mutex (Mutual Exclusion):** A binary lock ensuring that only one thread/process can access a shared resource at a given time.
- **Counting Semaphore:** An integer counter used to control access to a finite pool of shared resources (e.g., limiting concurrent database connections to 50).
- **Deadlock:** Occurs when Process 1 holds Lock A and waits for Lock B, while Process 2 holds Lock B and waits for Lock A. Both freeze indefinitely.
