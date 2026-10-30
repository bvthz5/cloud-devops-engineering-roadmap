# 02 — CPU Profiling and Bottleneck Analysis

When an application experiences high latency, determining whether CPU is actively executing code, waiting on kernel locks, or stalled on disk I/O is critical.

---

## 1. Demystifying Linux Load Average

```bash
uptime
# 10:45:01 up 42 days, 1 user,  load average: 8.50, 4.20, 2.10
```
The three numbers represent average system load over the last **1, 5, and 15 minutes**.
- **What is Load?** In Linux, load average is the count of tasks that are:
  1. Currently running on a CPU core (`R` state).
  2. Waiting in the CPU run queue ready to run (`R` state).
  3. **Waiting for uninterruptible disk or network I/O (`D` state)!**
- **Interpretation:** On an 8-core CPU server:
  - Load = `4.0`: System is 50% utilized.
  - Load = `8.0`: System is exactly at 100% capacity.
  - Load = `16.0`: System is saturated; processes are waiting in queue 50% of the time.

---

## 2. CPU Metric Breakdown (`mpstat -P ALL 1`)

```text
CPU    %usr   %nice    %sys %iowait   %irq   %soft  %steal  %guest  %idle
all   45.20    0.00   12.10   28.50   0.00    2.20    0.00    0.00  12.00
```
- **`%usr` (User Space):** Time executing user application code (Java, Python, Node.js). If high, optimize code or scale out.
- **`%sys` (Kernel System):** Time executing kernel system calls (network read/writes, memory allocations, context switches).
- **`%iowait` (I/O Wait):** CPU is idle, but threads are blocked waiting for disk I/O to complete. **High iowait means the disk is the bottleneck, NOT the CPU!**
- **`%steal`:** In virtual machines / cloud (AWS/GCP), % of time the hypervisor stole CPU cycles to serve other noisy neighbor VMs.

---

## 3. Pinpointing Rogue Processes (`pidstat -u 1`)

```bash
# Show top CPU-consuming processes every second with thread breakdowns
pidstat -u 1 5
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Linux Performance Methodologies USE and RED](./01-Linux-Performance-Methodologies-USE-and-RED.md) | [Index](../../../README.md) | [03 - Memory Tuning Swap PageCache and OOM →](./03-Memory-Tuning-Swap-PageCache-and-OOM.md) |
