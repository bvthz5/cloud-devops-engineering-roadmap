# 04 — Threads, Multithreading, and CPU Scheduling

---

## 1. What Is a Thread?

A **Thread** (often called a **Lightweight Process (LWP)** in Linux) is the smallest sequence of programmed instructions that can be managed independently by the operating system kernel scheduler.

In Linux, threads and processes are both represented internally by the same kernel data structure: **`struct task_struct`**. What distinguishes a thread from an independent process is whether it shares its memory address space and resources with other tasks.

```text
PROCESS A (Isolated Address Space)       PROCESS B (Multi-Threaded Process)
+-------------------------------+       +-------------------------------+
| Address Space: 0x0000 - 0x7FFF|       | Address Space: 0x0000 - 0x7FFF|
| [Code] [Data] [Heap]          |       | [Code] [Data] [Heap] (Shared) |
|                               |       |                               |
| Single Thread Execution       |       | Thread 1 Stack | Thread 2 Stack|
| [Private Stack & Registers]   |       | [Registers 1]  | [Registers 2] |
+-------------------------------+       +-------------------------------+
```

---

## 2. Process vs Thread: Deep Architectural Comparison

| Dimension | Process | Thread |
| :--- | :--- | :--- |
| **Address Space** | Private & Isolated (separate page tables) | Shared with other threads in the same process |
| **Creation System Call** | `fork()` or `clone(SIGCHLD, 0)` | `clone(CLONE_VM \| CLONE_FS \| CLONE_FILES \| CLONE_SIGHAND)` |
| **Creation Overhead** | Medium (~100–250 µs) | Low (~10–30 µs) |
| **Context Switch Cost** | Expensive (invalidates TLB, switches CR3 page root) | Inexpensive (shares page table, retains TLB entries) |
| **Memory Isolation** | High (Crash in one process cannot corrupt another) | Zero (A segfault in one thread kills the entire process) |
| **Communication (IPC)** | Complex (Pipes, Sockets, Shared Memory, IPC) | Direct (Reads and writes to shared global/heap memory) |
| **File Descriptors** | Independent copy | Shared table (closing FD in Thread 1 closes it for all) |

---

## 3. Concurrency vs Parallelism

```text
CONCURRENCY (Single Core, Time-Sliced)
Core 0: [ Thread 1 ][ Thread 2 ][ Thread 1 ][ Thread 2 ] ──► (Time)
        Progress is interleaved via rapid context switching.

PARALLELISM (Multi-Core, Simultaneous)
Core 0: [ Thread 1 ───────────────────────────────► ]
Core 1: [ Thread 2 ───────────────────────────────► ]
        Both threads execute physical machine instructions simultaneously.
```

- **Concurrency:** Managing multiple tasks by interleaving their execution over time.
- **Parallelism:** Executing multiple computational tasks at the exact same physical instant on separate hardware cores.

---

## 4. The Linux CPU Scheduler

The **CPU Scheduler** is the kernel subsystem responsible for deciding which runnable thread (`TASK_RUNNING`) receives access to a physical CPU core, and for how long.

### Modern Linux Schedulers
1. **Completely Fair Scheduler (CFS):** The standard Linux scheduler from kernel 2.6.23 to 6.5. Models an "ideal multi-tasking CPU" using red-black trees and tracking **Virtual Runtime (`vruntime`)**. Tasks with the lowest `vruntime` are selected next.
2. **EEVDF (Earliest Eligible Virtual Deadline First):** Introduced in Linux Kernel 6.6 as the replacement for CFS. Retains latency guarantees and fair queuing while drastically reducing scheduling jitter for latency-critical audio, gaming, and container workloads.

---

## 5. Scheduling Policies & Algorithms

Linux supports multiple scheduling policies categorized by workload type:

```text
+-------------------------------------------------------------+
|                  Linux Scheduling Policies                  |
+-------------------------------------------------------------+
| Real-Time (RT) Policies:                                    |
|   - SCHED_FIFO  : First-In, First-Out (runs until yield)    |
|   - SCHED_RR    : Round-Robin with fixed time slice         |
|   - SCHED_DEADLINE : Guaranteed completion deadlines        |
+-------------------------------------------------------------+
| Normal / Best-Effort Policies:                              |
|   - SCHED_OTHER (SCHED_NORMAL) : Default CFS / EEVDF        |
|   - SCHED_BATCH : Optimized for uninterrupted batch jobs    |
|   - SCHED_IDLE  : Extremely low priority background tasks   |
+-------------------------------------------------------------+
```

### Preemptive Scheduling & Time Slices
- **Preemptive:** The kernel can interrupt a currently executing process at any time (e.g., when its time slice expires or a higher-priority task wakes up) to give the CPU to another task.
- **Time Slice (Quantum):** The duration a thread is permitted to hold a CPU before being preempted. Under CFS, this is dynamic, scaling proportionally to the number of runnable tasks and their **Nice Value**.

### Process Priority: Nice Values
- Priority ranges from **`-20` (Highest priority, least nice)** to **`+19` (Lowest priority, most nice)**.
- Default nice value is `0`.
- Modifying nice value:
  ```bash
  # Start a backup script with lowest priority
  nice -n 19 tar -czf backup.tar.gz /var/data
  
  # Elevate priority of a running critical process
  sudo renice -n -10 -p 4512
  ```

---

## 6. Context Switching: Voluntary vs Involuntary

A **Context Switch** occurs when the CPU halts execution of one thread and resumes execution of another.

```text
+-------------------------------------------------------------+
|                 Context Switch Categories                   |
+-------------------------------------------------------------+
| Voluntary Context Switch:                                   |
|   The thread voluntarily relinquishes the CPU because it is |
|   blocked waiting for an unavailable resource (e.g., disk   |
|   read, incoming network packet, or sleeping on a mutex).   |
+-------------------------------------------------------------+
| Involuntary Context Switch:                                 |
|   The thread was forcibly preempted by the kernel scheduler |
|   because its allocated time slice expired or a higher      |
|   priority task became runnable.                            |
+-------------------------------------------------------------+
```

### Inspecting Context Switches
```bash
# Monitor system-wide context switches per second
vmstat 1

# Inspect context switches for a specific process (PID 1204)
pidstat -w -p 1204 1 5
```
Output columns:
- `cswch/s`: Voluntary context switches per second.
- `nvcswch/s`: Non-voluntary (involuntary) context switches per second.
- **High `nvcswch/s`** indicates CPU starvation: too many threads competing for too few CPU cores.

---

## 7. CPU Affinity & Processor Pinning

By default, the Linux scheduler dynamically migrates threads between CPU cores to balance thermal load and throughput. However, moving a thread to a different core flushes its **L1/L2 hardware caches**, causing cache misses.

**CPU Affinity** binds a process or thread strictly to a specific physical core or set of cores.

```text
[ Core 0: Pinned ] ──► [ Nginx Worker 1 ] (100% Cache Hit Rate)
[ Core 1: Pinned ] ──► [ Nginx Worker 2 ] (100% Cache Hit Rate)
[ Core 2: Dynamic] ──► [ Other OS Tasks / Cron / SSH ]
[ Core 3: Dynamic] ──► [ Other OS Tasks / Cron / SSH ]
```

### Managing CPU Affinity with `taskset`
```bash
# Launch a high-throughput script pinned to CPU cores 0 and 1
taskset -c 0,1 python3 worker.py

# Check affinity of a running process
taskset -cp 3120

# Re-assign running process to core 2 only
sudo taskset -cp 2 3120
```

---

## SRE Performance Summary
- Keep active thread counts aligned with physical core counts for CPU-bound tasks.
- Avoid oversubscribing threads (e.g., 200 threads on a 2-core VM); this drives high involuntary context switches (`nvcswch/s`), spending valuable CPU cycles switching tasks rather than doing actual work.
- Use CPU pinning (`taskset` or Kubernetes `CPU Manager static policy`) for latency-critical database engines and network proxies.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Process Lifecycle fork exec States and Zombies](./03-Process-Lifecycle-fork-exec-States-and-Zombies.md) | [Index](../../../README.md) | [05 - OS Memory Management Paging Swap and mmap →](./05-OS-Memory-Management-Paging-Swap-and-mmap.md) |
