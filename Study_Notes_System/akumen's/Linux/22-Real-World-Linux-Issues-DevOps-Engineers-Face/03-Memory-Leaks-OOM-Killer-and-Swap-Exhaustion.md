# 03 — Memory Leaks, OOM Killer, and Swap Exhaustion

## 1. Scenario
A backend database or Java process randomly crashes during peak workload without writing any application stack traces or error logs. System monitors indicate RAM usage climbed to 99% before the crash.

```text
Scenario
   ↓
Symptoms: Process suddenly disappears/terminates, Exit code 137 (SIGKILL), swap usage 100%
   ↓
What could cause it? Memory leak in application code, unconstrained heap memory, kernel OOM invocation
   ↓
Diagnostic commands: free -h, dmesg -T | grep -i oom, vmstat 1 5, ps aux --sort=-%mem
   ↓
Understand output: "Out of memory: Kill process 12450 (java) score 850"
   ↓
Find root cause: Kernel OOM-Killer terminated Java process after host memory and swap ran out
   ↓
Fix / mitigate: Adjust JVM -Xmx max memory heap limits, set swap space, adjust oom_score_adj
   ↓
Verify: Memory utilization stabilizes within limits; process remains healthy under load
   ↓
Prevent recurrence: Configure cgroup memory limits in container definition
```

## 2. Diagnosing Out of Memory (OOM) Invocations

```bash
# Check memory allocation
free -h

# Search kernel ring buffer logs for OOM invocation messages
dmesg -T | grep -iE 'out of memory|killed process'
```

### Sample OOM Kernel Log Output Analysis
```text
[Thu Sep 17 21:04:12 2026] Out of memory: Kill process 12450 (java) score 892 or sacrifice child
[Thu Sep 17 21:04:12 2026] Killed process 12450 (java) total-vm:8450120kB, anon-rss:3890120kB, file-rss:0kB, shmem-rss:0kB
```

- **`anon-rss`**: Anonymous Resident Set Size (physical RAM allocated to heap/stack).
- **`score 892`**: The `oom_score` calculated by the kernel (processes with highest score are killed first when memory runs out).

## 3. Protecting Critical Processes from OOM-Killer (`oom_score_adj`)
To protect essential daemons (like `sshd` or system monitor agents) from being killed by the OOM-killer:

```bash
# Adjust oom_score_adj (-1000 disables OOM killing completely for this PID)
echo -1000 > /proc/$(pgrep sshd)/oom_score_adj
```

## 4. Swap Thrashing Identification
When physical RAM runs out, the kernel pages inactive memory to disk swap space. If active memory is continuously swapped back and forth, the system suffers severe **Swap Thrashing** (high `%wa` CPU, frozen I/O).

```bash
# Monitor swap paging in/out per second (si / so columns)
vmstat 1 5
```
> If `si` (swap in) and `so` (swap out) values are continuously non-zero (>0), the system is thrashing!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - High CPU Usage](./02-High-CPU-Usage-and-Load-Average-Diagnosis.md) | [README](./README.md) | [04 - Disk Space Leak & Deleted Open Files](./04-Disk-Space-Exhaustion-and-Deleted-Open-Files.md) |
