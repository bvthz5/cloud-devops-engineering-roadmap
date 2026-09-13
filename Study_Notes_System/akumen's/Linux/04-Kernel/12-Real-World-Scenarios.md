# 12 - Real-World Scenarios & Production Kernel Incidents

Senior DevOps and Site Reliability Engineers must understand how kernel-level failures manifest in production cloud environments.

---

## 1. Scenario: CPU Soft Lockup on High-Traffic Nodes

### The Symptom:
A Kubernetes node becomes unresponsive. In AWS CloudWatch or the host console, the kernel logs:
```
[Sun Sep 13 14:10:22 2026] watchdog: BUG: soft lockup - CPU#3 stuck for 22s! [kworker/3:1:1042]
```

### The Architectural Cause:
A **Soft Lockup** occurs when a CPU core is executing inside Kernel Space (Ring 0) and gets trapped in a spinlock or tight loop for longer than the watchdog threshold (default: **20 seconds**) without yielding to the scheduler or responding to timer interrupts.
- **Why it happens:** Hardware driver deadlock, buggy kernel module, or severe disk I/O lock contention.
- **Watchdog Mechanism:** The Linux kernel runs a dedicated high-priority watchdog thread per CPU core. If the core fails to update its watchdog timestamp for 20 seconds, the kernel detects the freeze and logs the backtrace.

```bash
# SRE Action: Inspect lockup threshold & configure automatic reboot on lockup:
$ sysctl kernel.watchdog_thresh
kernel.watchdog_thresh = 20

# Force kernel to panic and reboot immediately on soft lockup (triggering K8s node failover):
$ sudo sysctl -w kernel.softlockup_panic=1
```

---

## 2. Scenario: The "Phantom" Kernel SLAB Memory Leak

### The Symptom:
Monitoring alerts report that a 64 GB database server has **60 GB of RAM used (94%)**.
You log in and sum the Resident Set Size (RSS) of all running processes in `ps aux`:
```bash
$ ps aux | awk '{sum+=$6} END {print sum/1024 " MB"}'
12240 MB  # Only 12 GB is accounted for by user applications!
```
Where did the missing **48 GB of physical RAM** go?

### The Architectural Cause: Kernel SLAB Exhaustion
`ps` only tracks **User Space** process memory. It has zero visibility into memory allocated directly by the kernel!
A misbehaving software component (such as a container runtime creating millions of temporary mount namespaces or dangling dentry paths) leaked memory inside the **Kernel SLAB cache**.

### Diagnostic & Triage:
```bash
# 1. Inspect kernel slab memory in /proc/meminfo:
$ cat /proc/meminfo | grep -E "Slab|SReclaim|SUnreclaim"
Slab:           48120940 kB   <-- 48 GB consumed by the Kernel!
SReclaimable:   46102140 kB
SUnreclaimable:  2018800 kB

# 2. Identify the specific leaking kernel structure:
$ sudo slabtop -s c | head -n 10
  OBJS ACTIVE  USE OBJ SIZE  SLABS OBJ/SLAB CACHE SIZE NAME
481020 480100  99%    0.19K  23400       41     46100M dentry

# Notice: 'dentry' (directory entry cache) is consuming 46 GB of RAM!

# 3. Emergency Remediation: Instruct kernel to drop reclaimable caches:
$ sudo bash -c 'echo 2 > /proc/sys/vm/drop_caches'
```

---

## 3. Scenario: Zero-Downtime Kernel Live Patching

### The Problem:
A critical zero-day privilege escalation vulnerability is discovered in the Linux kernel (e.g., Dirty COW, Dirty Pipe). Security compliance mandates patching all 500 production database nodes within 24 hours, but rebooting each node would cause unacceptable customer downtime.

### The Modern Solution: Kernel Live Patching (`kpatch` / Livepatch)
Modern Linux kernels support **Live Patching** without rebooting the host:

```
Vulnerable Function:
[sys_foo()] ─── Normal execution (Contains security bug!)

Action: Kernel Live Patch Module Loaded via ftrace
[sys_foo()] ─── First instruction overwritten with: jmp [sys_foo_patched()]
                     │
                     ▼
[sys_foo_patched()] (Executes secured logic directly in RAM!)
```

Using tools like **Canonical Livepatch** or Red Hat **kpatch**, the kernel uses the internal `ftrace` function tracer to atomically redirect the entry point of the vulnerable function to a patched routine in RAM. The security vulnerability is closed instantly with **zero downtime and zero reboot required**.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Linux Distributions](./11-Linux-Distributions.md) | [README](./README.md) | [13 - Troubleshooting](./13-Troubleshooting.md) |
