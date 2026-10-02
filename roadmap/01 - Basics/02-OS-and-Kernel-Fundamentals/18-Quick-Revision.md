# 18 — Quick Revision & Reference Cheat Sheet

A condensed, high-yield reference guide summarizing Operating System and Kernel Fundamentals for rapid interview review and real-time production troubleshooting.

---

## 1. User Space vs Kernel Space Comparison

| Feature | User Space (Ring 3) | Kernel Space (Ring 0) |
| :--- | :--- | :--- |
| **Privilege Level** | Lowest (Restricted) | Highest (Unrestricted) |
| **CPU Instructions** | Standard user instructions only | All instructions (HALT, MMU, Port I/O, Interrupts) |
| **Memory Access** | Private, isolated virtual memory | Full physical memory + all process address spaces |
| **Crash Result** | Process crashes (`SIGSEGV`), OS survives | Catastrophic **Kernel Panic** / System reboot |
| **Examples** | Nginx, Python, Docker, Shell | Scheduler, Memory Manager, VFS, Netfilter, Drivers |

---

## 2. Process State Machine Quick Map

```text
       Created ──► [ TASK_RUNNING (R) ] ◄── Scheduled on CPU
                         │         ▲
             Waiting I/O │         │ I/O Ready
                         ▼         │
                  [ SLEEPING (S/D) ]
                         │
                    exit() called
                         │
                         ▼
                  [ ZOMBIE (Z) ] ──(Parent wait() called)──► [ DEAD / REAPED ]
```

- **`R`**: Running or runnable in CPU queue.
- **`S`**: Interruptible sleep (waiting for socket/timer/signal).
- **`D`**: Uninterruptible sleep (waiting on hardware disk I/O; **cannot be killed**).
- **`T`**: Stopped / paused via signal (`SIGSTOP` / `Ctrl+Z`).
- **`Z`**: Zombie / defunct (dead, holds PID waiting for parent `wait()`).

---

## 3. Essential Linux Signals Cheat Sheet

| Signal | Num | Catchable? | Default Action | DevOps / SRE Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **`SIGHUP`** | `1` | Yes | Terminate | Zero-downtime configuration reload (`kill -HUP <PID>`) |
| **`SIGINT`** | `2` | Yes | Terminate | Interactive terminal interrupt (`Ctrl+C`) |
| **`SIGQUIT`**| `3` | Yes | Core Dump | Force quit with memory core dump (`Ctrl+\`) |
| **`SIGKILL`**| `9` | **NO** | Terminate | Uncatchable force kill; kernel immediately reclaims process |
| **`SIGSEGV`**| `11`| Yes | Core Dump | Memory violation (segfault / invalid pointer) |
| **`SIGPIPE`**| `13`| Yes | Terminate | Attempted write to closed pipe/socket |
| **`SIGTERM`**| `15`| Yes | Terminate | Graceful shutdown signal (Kubernetes pod eviction) |
| **`SIGCHLD`**| `17`| Yes | Ignore | Notifies parent that child terminated (reaps zombies) |
| **`SIGSTOP`**| `19`| **NO** | Pause | Uncatchable execution freeze (`Ctrl+Z`) |
| **`SIGCONT`**| `18`| Yes | Resume | Resumes execution of a stopped process |

---

## 4. Linux Namespaces vs Control Groups (cgroups)

```text
+-------------------------------------------------------------+
|             How Linux Containers Actually Work              |
+-------------------------------------------------------------+
| Namespaces = ISOLATION (What a process can SEE)             |
|   ├─ PID     : Private process tree (App runs as PID 1)     |
|   ├─ NET     : Private network interfaces, routing & ports  |
|   ├─ MNT     : Private root filesystem & mount points       |
|   ├─ IPC     : Private shared memory & message queues       |
|   ├─ UTS     : Private hostname and domain                  |
|   ├─ USER    : Maps container root to unprivileged host UID |
|   └─ CGROUP  : Root directory of cgroups                    |
+-------------------------------------------------------------+
| cgroups = RESOURCE CONSTRAINTS (What a process can USE)     |
|   ├─ CPU     : CPU bandwidth, quotas, CFS periods           |
|   ├─ Memory  : Hard ceilings (memory.max) & soft reclaim    |
|   ├─ I/O     : Read/write IOPS and MB/s limits              |
|   └─ PIDs    : Maximum concurrent process limit             |
+-------------------------------------------------------------+
```

---

## 5. File Descriptors & Streams Reference

| File Descriptor | Name | Purpose | Default Destination |
| :--- | :--- | :--- | :--- |
| **`0`** | `stdin` | Standard Input | Keyboard / Input Pipe |
| **`1`** | `stdout`| Standard Output | Terminal Console / Screen |
| **`2`** | `stderr`| Standard Error | Terminal Console (Unbuffered) |

- **Redirect both stdout and stderr to a file:** `command > output.log 2>&1` or `command &> output.log`
- **Discard all output completely:** `command > /dev/null 2>&1`
- **Check open file descriptors for PID:** `ls -l /proc/<PID>/fd/`

---

## 6. Essential OS & Kernel Diagnostic Commands

| Task | Command |
| :--- | :--- |
| **Trace System Calls** | `strace -c -p <PID>` |
| **View Process Tree & PIDs** | `pstree -p` |
| **Inspect Unkillable D-State Stack** | `cat /proc/<PID>/stack` |
| **Find Open Files & Network Sockets**| `lsof -i :8080` / `lsof -p <PID>` |
| **Audit Kernel Ring Buffer (OOM/Hardware)**| `dmesg -T \| grep -E -i "oom\|error"` |
| **Display Active Kernel Parameters** | `sysctl -a` |
| **Check Swappiness & Memory Overcommit** | `sysctl vm.swappiness vm.overcommit_memory` |
| **Monitor Context Switches & Run Queues** | `vmstat 1` |
| **Per-Process CPU & Memory Profiling** | `pidstat -u -r 1` |
| **Check Inode Utilization on Filesystem** | `df -ih` |
| **Service Status & Real-time Logs** | `systemctl status <svc>` / `journalctl -u <svc> -f` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [17 - MCQ](./17-MCQ.md) | [README](./README.md) | - |
