# 12 - Troubleshooting & System Incident Playbooks

Operating system fundamentals dictate how to triage systemic incidents such as runaway load averages, swap thrashing, zombie leaks, and file descriptor exhaustion.

---

## 🚨 Incident 1: High Load Average With Low CPU Utilization!

### The Symptom:
A 4-core server reports a crushing **Load Average of 45.00**, but `top` reports CPU idle time is **90%**! How can the load average be 45 when the CPU is barely working?

### The OS Mechanism:
In Linux, **Load Average** does not measure CPU usage alone. It calculates the running average of processes that are:
1. Currently executing on a CPU core (`R`).
2. Waiting in the CPU ready run-queue (`R`).
3. **Sleeping in Uninterruptible Disk I/O State (`D`)!**

```
Load Average = (Active Running Processes) + (Processes Blocked on Disk I/O)
```

If your disk array or network NFS share hangs, 45 database queries can become blocked in state `D` waiting for disk blocks. The CPU sits completely idle, but the Load Average shoots up to 45!

```bash
# 1. Inspect processes in state 'D':
$ ps aux | awk '$8 ~ /D/'

# 2. Check disk I/O metrics to find the saturated disk:
$ iostat -xz 1 5
# Look at the '%util' column: if %util = 100%, disk hardware is saturated!
```

---

## 🚨 Incident 2: Memory Thrashing & Swap Storms

### The Symptom:
System responsiveness drops to zero. SSH keystrokes take 10 seconds to echo. Disk activity LEDs flash furiously.

### The OS Cause:
Physical RAM is depleted. The kernel is desperately paging anonymous memory out to disk and reading pages back in simultaneously (**Thrashing**).

### Diagnostic & Resolution:
```bash
# 1. Observe swap in (si) and swap out (so) rates:
$ vmstat 1
procs -----------memory---------- ---swap-- -----io----
 r  b   swpd   free   buff  cache   si   so    bi    bo
 2 12 4194304  32100   8200  42100 8420 9100  9200 10200
# Notice: Huge values in 'si' and 'so' (> 8000 KB/s)!

# 2. Find which processes are consuming physical RAM:
$ ps aux --sort=-%mem | head -n 5

# 3. Emergency Fix: Kill the rogue memory consumer:
$ sudo kill -9 <PID>

# 4. Long-term OS Tuning: Lower swappiness to favor page cache eviction over swapping:
$ sudo sysctl vm.swappiness=10
```

---

## 🚨 Incident 3: File Descriptor Exhaustion ("Too many open files")

### The Symptom:
A high-throughput Nginx or Redis server starts rejecting incoming network requests with:
```
accept4() failed: Too many open files (EMFILE)
```

### The OS Cause:
Every open socket, disk file, pipe, and device consumes one **File Descriptor (FD)**. Linux enforces two limits:
1. **Per-process limit (`ulimit -n`):** Default is often only 1,024.
2. **System-wide limit (`/proc/sys/fs/file-max`).**

### Diagnostic & Resolution:
```bash
# 1. Check current per-process limit for open files:
$ ulimit -n
1024

# 2. Count active open file descriptors for PID 2841:
$ sudo ls -l /proc/2841/fd | wc -l
1024   <-- Hit the ceiling!

# 3. Check system-wide file allocation:
$ cat /proc/sys/fs/file-nr
14208    0    9223372036854775807
# (Allocated FDs, Allocated Unused, Maximum Allowed)

# 4. Permanent Fix: Raise limits in /etc/security/limits.conf:
*    soft    nofile    65535
*    hard    nofile    65535

# For systemd service units, add to service override:
LimitNOFILE=65535
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Real World Scenarios](./11-Real-World-Scenarios.md) | [README](./README.md) | [13 - Interview QA](./13-Interview-QA.md) |
