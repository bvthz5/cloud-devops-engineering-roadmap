# 14 — OS & Kernel Troubleshooting Guide

Systematic, diagnostic workflows to debug complex operating system anomalies, kernel hangs, unkillable processes, and resource starvation.

---

## 1. Diagnostic Decision Tree for Kernel & OS Outages

```text
                  [ Operating System Anomaly Detected ]
                                    │
       ┌────────────────────────────┼────────────────────────────┐
       ▼                            ▼                            ▼
[ High %sys CPU Time ]     [ Unkillable Processes ]      [ Out of Memory / OOM ]
       │                            │                            │
Syscall Storm or Lock       State 'D' (Disk Wait)        Check `dmesg -T`
Trace with `perf top`       Check `/proc/$PID/stack`     Tune `oom_score_adj`
Inspect with `strace -c`    Identify stuck NFS/NVMe      Check container limits
```

---

## 2. Issue 1: High Kernel/System CPU Time (`%sys`)

### Symptoms
- In `top` or `mpstat`, CPU `%sys` is elevated (> 30-50%), while application `%usr` is low.
- Total application throughput drops significantly despite high CPU activity.

### Root Causes
1. **Excessive System Call Overhead:** An application is invoking millions of small syscalls per second (e.g., calling `gettimeofday()` or 1-byte `read()` loops).
2. **Kernel Lock Contention:** Multiple threads are fighting for a shared spinlock or futex in kernel space.
3. **Severe Page Fault Rate:** Rapid memory allocations triggering major page faults or page table walks.

### Diagnostic Workflow
```bash
# 1. Identify which processes are consuming high %sys
pidstat -u 1 5 | sort -k 5 -r | head -n 10

# 2. Sample live kernel functions consuming CPU cycles (using perf)
sudo perf top

# Look at top symbols:
# If you see `spin_lock` -> Kernel locking contention
# If you see `clear_page` -> Massive memory zeroing / paging churn
# If you see `sys_read`   -> Syscall I/O loop

# 3. Trace syscall distribution of the culprit PID
sudo strace -c -p <PID>
```

---

## 3. Issue 2: Unkillable Processes in `D` State (TASK_UNINTERRUPTIBLE)

### Symptoms
- A process cannot be terminated, even using `kill -9 <PID>` as root.
- The process status column in `ps` reports `D` (or `D+`).
- System load average increases continuously even when the CPU is idle.

### Root Cause
The process is blocked inside a kernel system call waiting on hardware I/O (usually an un-responsive NFS mount, a dead NVMe drive, or a locked hardware resource). Because the process is in **Uninterruptible Sleep**, the kernel prevents signal delivery until the hardware I/O returns.

### Step-by-Step Diagnostic Procedure
```bash
# 1. Locate all processes currently stuck in D state
ps -eo pid,user,stat,wchan:20,comm | grep -E " D"

# 2. Inspect the exact kernel call stack where the process is frozen
sudo cat /proc/<PID>/stack
```

Sample `/proc/<PID>/stack` Output:
```text
[<0>] nfs_wait_bit_killable+0x34/0x90 [nfs]
[<0>] nfs4_proc_lookup_mountpoint+0x12a/0x240 [nfsv4]
[<0>] lookup_slow+0xa4/0x170
[<0>] walk_component+0x12b/0x1a0
[<0>] path_lookupat+0x6d/0x1a0
[<0>] do_filp_open+0x8e/0x130
[<0>] do_sys_openat2+0x9b/0x160
[<0>] __x64_sys_openat+0x54/0x90
[<0>] do_syscall_64+0x5b/0x90
```
- **Analysis:** The stack above reveals the process is stuck inside `nfs_wait_bit_killable`, proving that a hanging network filesystem (NFS) mount is causing the freeze.

### Remediation
1. You **cannot kill the process directly**—sending `SIGKILL` will simply remain queued in the kernel.
2. Unmount the broken NFS mount with force and lazy flags:
   ```bash
   sudo umount -f -l /mnt/broken_nfs
   ```
3. If the hang is caused by a failed physical local NVMe drive, rebooting the machine or power-cycling the host is the only resolution.

---

## 4. Issue 3: Inotify Limit Reached (`ENOSPC`)

### Symptoms
- Tools watching files (e.g., Webpack, Vite, IDEs, or Kubernetes `tail` log agents) crash on startup with:
  `Error: ENOSPC: System limit for number of file watchers reached`.

### Root Cause
Linux restricts the number of filesystem directories and files a user can monitor using the `inotify` subsystem. The default limit is usually `8192`.

### Diagnostic & Remediation
```bash
# 1. Check active inotify watchers count
cat /proc/sys/fs/inotify/max_user_watches

# 2. Check how many watchers are currently being used across all processes
find /proc/*/fd/* -type l -lname 'anon_inode:inotify' 2>/dev/null | cut -d/ -f3 | xargs -I '{}' -- ps --no-headers -u -p '{}'

# 3. Increase the limit immediately
sudo sysctl -w fs.inotify.max_user_watches=524288
sudo sysctl -w fs.inotify.max_user_instances=8192

# 4. Make persistent in /etc/sysctl.d/99-inotify.conf
echo "fs.inotify.max_user_watches=524288" | sudo tee -a /etc/sysctl.d/99-inotify.conf
```

---

## 5. Issue 4: Analyzing an Out-Of-Memory (OOM) Killer Incident

### Objective
Determine which process caused an OOM crash, how much memory was available, and why the kernel chose that specific process to kill.

### Post-Mortem Commands
```bash
# 1. Extract the complete OOM killer report from kernel logs
sudo dmesg -T | grep -A 25 -i "invoked oom-killer"

# 2. View which process was terminated and its memory footprint
# Example output:
# [Fri Oct  2 07:35:10 2026] Out of memory: Killed process 15201 (java)
# total-vm:8452120kB, anon-rss:4120150kB, file-rss:0kB, shmem-rss:0kB
```

### Explaining OOM Badness Calculation
The kernel scores every process (`/proc/[PID]/oom_score`) based on:
1. Percentage of physical RAM consumed (`anon-rss`).
2. Duration of execution (short-lived hogs are prioritized over long-running system daemons).
3. Privilege (root processes receive a slight discount).
4. Manual adjustment via **`/proc/[PID]/oom_score_adj`** (range `-1000` to `+1000`).

### Protecting Critical Daemons from OOM Killer
```bash
# Make SSH daemon completely immune to OOM killer (-1000)
echo -1000 | sudo tee /proc/$(pgrep -o sshd)/oom_score_adj
```

---

## Summary Diagnostic Command Matrix

| Problem | Primary Tool | Command | What to Look For |
| :--- | :--- | :--- | :--- |
| **Syscall Storm** | `strace` | `strace -c -p <PID>` | Excessive syscalls per second (> 50,000/s) |
| **Kernel Locks** | `perf` | `sudo perf top` | Spinlock or mutex contention in kernel |
| **Unkillable 'D' State**| `procfs` | `cat /proc/<PID>/stack` | Driver or NFS hang in kernel stack |
| **Zombie Accumulation** | `ps` | `ps aux \| grep 'Z'` | Parent failing to call `wait()` |
| **File Descriptor Leak**| `lsof` | `lsof -p <PID> \| wc -l` | Rapidly growing open FD count |
| **OOM Termination** | `dmesg` | `dmesg -T \| grep -i oom` | Memory footprint and killed PID |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 13 - Real World Scenarios](./13-Real-World-Scenarios.md) | [Index](../../../README.md) | [15 - Interview QA →](./15-Interview-QA.md) |
