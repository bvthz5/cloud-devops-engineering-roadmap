# 02 — High CPU Usage and Load Average Diagnosis

## 1. Scenario
A production web application reports slow response times and timeouts. Monitoring alerts flag **High Load Average (16.0)** on a 4-CPU core Linux instance.

```text
Scenario
   ↓
Symptoms: API responses slow, HTTP 504 Gateway Timeouts, high CPU alerts
   ↓
What could cause it? Runaway CPU process, excessive thread contention, I/O wait, hardware interrupts
   ↓
Diagnostic commands: uptime, top, htop, pidstat 1 5, mpstat -P ALL 1
   ↓
Understand output: CPU %sy high vs %us high vs %wa high
   ↓
Find root cause: Unoptimized loop in Node.js process (PID 4821) consuming 398% CPU
   ↓
Fix / mitigate: Kill runaway process or lower priority with renice / cpulimit / restart service
   ↓
Verify: Load average drops back below 4.0; HTTP latency returns to normal
   ↓
Prevent recurrence: Implement CPU cgroup resource limits in Docker/Kubernetes
```

## 2. Diagnostic Commands & Output Analysis

```bash
# Check load average across 1, 5, and 15 minute intervals
uptime
# Output: 22:15:00 up 45 days,  2:10,  2 users,  load average: 16.20, 12.10, 8.40

# Check total CPU core count
nproc
# Output: 4

# Detailed CPU states inspection
top -bn1 | head -n 5
```

### Understanding Top CPU Breakdown Symbols
- **`%us` (user)**: CPU time spent running unprivileged user space processes (e.g. Node.js, Python, Java).
- **`%sy` (system)**: CPU time spent running kernel system calls (indicates excessive context switching or file/network I/O syscalls).
- **`%wa` (iowait)**: CPU time spent waiting for disk/network I/O operations to complete.
- **`%hi` / `%si`**: Time servicing hardware/software interrupts.

## 3. Isolating Runaway Process with `pidstat`

```bash
# Monitor per-process CPU usage every 1 second for 5 iterations
pidstat -u 1 5
```

```text
10:15:01 AM   UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
10:15:02 AM  1001      4821  392.00    6.00    0.00    0.00  398.00     2  node
```

## 4. Remediation Options
```bash
# Option A: Gracefully reload target service
systemctl reload web-app

# Option B: Dynamically lower process priority (higher nice value = lower priority)
renice -n 19 -p 4821

# Option C: Terminate rogue process if non-essential
kill -15 4821
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Troubleshooting Mindset](./01-Linux-Troubleshooting-Mindset-and-Workflow.md) | [README](./README.md) | [03 - Memory Leaks & OOM Killer](./03-Memory-Leaks-OOM-Killer-and-Swap-Exhaustion.md) |
