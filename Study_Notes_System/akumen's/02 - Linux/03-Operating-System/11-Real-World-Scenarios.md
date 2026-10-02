# 11 - Real-World Scenarios & Production Architectures

Applying operating system theory allows engineers to diagnose distributed system deadlocks, container CPU throttling, and memory leaks.

---

## 1. Deadlock in Production Systems: The 4 Coffman Conditions

In operating systems and databases, a **deadlock** occurs when a set of processes are blocked because each process is holding a resource and waiting for another resource acquired by some other process in the set.

```
Process 1 (Holds Lock A) ────── Wants Lock B ──────► Process 2 (Holds Lock B)
      ▲                                                    │
      └────────────────── Wants Lock A ────────────────────┘
                   (DEADLOCK: Neither can proceed!)
```

### The 4 Mandatory Coffman Conditions:
A deadlock can occur **if and only if all four** of the following conditions hold simultaneously:
1. **Mutual Exclusion:** At least one resource must be held in a non-shareable mode (only one process can use it at a time).
2. **Hold and Wait:** A process must currently hold at least one resource and be requesting additional resources held by other processes.
3. **No Preemption:** Resources cannot be forcibly confiscated; they can only be released voluntarily by the holding process.
4. **Circular Wait:** A closed chain of processes exists, where each process holds a resource needed by the next process in the chain.

### SRE / DevOps Mitigation Strategy:
- **Lock Ordering:** Enforce global deterministic lock ordering in code (always acquire Lock A before Lock B).
- **Lock Timeouts:** Never wait indefinitely (`pthread_mutex_timedlock` or SQL lock timeouts `SET lock_timeout = '5s'`).
- **Deadlock Detection:** Databases (PostgreSQL/MySQL) run background cycle-detection algorithms on the wait-for graph and abort the cheaper transaction with a rollback.

---

## 2. Kubernetes CPU Throttling: How the CFS Quota Works

### The Symptom:
A developer allocates `resources.limits.cpu: "500m"` (0.5 CPU cores) to a microservice in Kubernetes. Even though overall cluster CPU usage is low, API response latency spikes from 10ms to 800ms!

### The OS Mechanism:
Linux implements container CPU limits using the kernel's **Completely Fair Scheduler (CFS) Quotas**:
- The kernel defines a period window: **`cpu.cfs_period_us = 100,000` microseconds (100 ms)**.
- A limit of `500m` translates to **`cpu.cfs_quota_us = 50,000` microseconds (50 ms)**.

```
100ms CFS Period Window:
┌──────────────────────────────┬──────────────────────────────┐
│ First 50ms: Active CPU       │ Next 50ms: HARD THROTTLED    │
│ App uses all 50ms allowance  │ App paused by kernel CFS     │
│ at the start of the burst    │ Latency increases 10x!       │
└──────────────────────────────┴──────────────────────────────┘
```

If the application fires multi-threaded requests and exhausts its 50ms budget within the first 10ms of the window, **the Linux kernel completely freezes the process for the remaining 90ms!**

```bash
# Diagnosing CFS throttling inside a container or cgroup:
$ cat /sys/fs/cgroup/cpu.stat
nr_periods 14205
nr_throttled 6812      <-- 48% of scheduling periods were throttled!
throttled_usec 481023901
```

---

## 3. Demystifying Memory Metrics: VSZ vs. RSS

A common source of confusion when monitoring memory leaks is the difference between **VSZ** and **RSS** in `ps` and `top`:

```
┌─────────────────────────────────────────────────────────────┐
│ Virtual Size (VSZ): Total Virtual Address Space Allocated   │
│ (Includes shared libraries, mapped files, and uncommitted   │
│  virtual memory allocations).                               │
│                                                             │
│       ┌──────────────────────────────────────────────┐      │
│       │ Resident Set Size (RSS): Physical RAM Used   │      │
│       │ (The actual number of 4 KB pages mapped to   │      │
│       │  physical DRAM frames in physical memory).   │      │
│       └──────────────────────────────────────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

- **VSZ (Virtual Memory Size):** How much memory the program *thinks* it has requested. If a program calls `malloc(10 * 1024 * 1024 * 1024)` (10 GB) but never writes to it, VSZ will increase by 10 GB, but **RSS will remain 0 MB** because of demand paging!
- **RSS (Resident Set Size):** The true physical RAM currently consumed by the process.
- **Rule of Thumb:** A high VSZ is usually harmless. A continuously climbing **RSS** indicates an application memory leak that will eventually trigger the OOM killer.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Linux Distributions](./10-Linux-Distributions.md) | [README](./README.md) | [12 - Troubleshooting](./12-Troubleshooting.md) |
