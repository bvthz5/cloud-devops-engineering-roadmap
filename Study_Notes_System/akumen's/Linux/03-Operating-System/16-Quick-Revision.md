# 16 - Quick Revision & 5-Minute OS Cheat Sheet

Use this cheat sheet for rapid pre-interview revision and conceptual review.

---

## ⚡ 4-Tier Abstraction Model
```
User ➔ Application ➔ Operating System ➔ Hardware
```

---

## 🏛️ The 7 Core Functions of an OS

| Function | Primary Role | Key Linux Subsystem |
|---|---|---|
| **CPU Management** | Schedules processes fairly across cores | CFS Scheduler, `task_struct` |
| **Memory Management** | Maps virtual pages to physical RAM frames | MMU, Paging, Page Cache, Swap |
| **File Management** | Organizes structured data in directories | Virtual Filesystem (VFS), Inodes |
| **Device I/O** | Buffers, caches, and coordinates hardware | DMA, Block/Char Drivers, udev |
| **Security & Protection**| Enforces access controls and boundaries | UID/GID, DAC, MAC (SELinux), Cgroups |
| **Resource Accounting**| Tracks CPU time, memory, and task stats | `/proc`, Cgroups v2, `psacct` |
| **Error Handling** | Traps faults, routes signals, logs panics | Signals (`SIGSEGV`), Kernel Panic |

---

## 🔄 Process States & Transitions
- **`New` ➔ `Ready`:** Process created; waiting in CPU run queue.
- **`Ready` ➔ `Running`:** Scheduler allocates CPU core.
- **`Running` ➔ `Waiting (Blocked)`:** Process requests I/O or sleep timer.
- **`Waiting` ➔ `Ready`:** I/O completes; process re-enters run queue.
- **`Running` ➔ `Terminated`:** Process calls `exit()`.
- **`Zombie`:** Terminated process whose parent has not called `wait()`.
- **`Orphan`:** Process whose parent died; adopted by PID 1 (`systemd`).

---

## 💾 Memory Management Essentials
- **Virtual Page:** 4 KB virtual memory block.
- **Physical Frame:** 4 KB physical DRAM memory block.
- **MMU:** CPU hardware translating virtual addresses to physical frames.
- **TLB:** Hardware cache of recent page translations.
- **Page Fault:** CPU trap when a page is accessed whose present bit is 0.
- **Thrashing:** System spending more time swapping than executing code.

---

## 🔒 The 4 Coffman Deadlock Conditions
1. **Mutual Exclusion:** Exclusive resource access.
2. **Hold and Wait:** Holding a resource while requesting another.
3. **No Preemption:** Resources cannot be forcibly seized.
4. **Circular Wait:** Closed chain of circular dependencies.

---

## 🐧 Linux Distribution Family Quick Reference

| Family | Package Format | Package Manager | Primary Usage |
|---|:---:|:---:|---|
| **Debian / Ubuntu** | `.deb` | `apt` / `dpkg` | Cloud servers, web hosting, general DevOps |
| **Red Hat (RHEL/Rocky)**| `.rpm` | `dnf` / `rpm` | Enterprise banking, government, strict compliance |
| **Alpine Linux** | `.apk` | `apk` | Minimal Docker container base images (~5 MB) |
| **Immutable (Talos/Flatcar)**| Container images | API / sysext | Cloud-native Kubernetes host operating systems |

---

## 🛠️ High-Frequency Diagnostic Commands

```bash
# Check load average (1, 5, 15 min):
uptime

# Track memory, swap in/out, and context switches:
vmstat 1

# View process hierarchy tree:
pstree -p

# Find zombie processes:
ps aux | grep -E "Z|defunct"

# Inspect active hardware interrupts:
cat /proc/interrupts
```
