# 06 - Kernel Subsystem: Memory Management & Allocation

The Linux kernel memory management subsystem coordinates physical DRAM, CPU cache lines, virtual memory translations, and non-volatile swap storage.

---

## 1. Physical Page Allocation: The Buddy Allocator

Physical RAM is partitioned into contiguous 4 KB blocks called **Page Frames**.
To manage the allocation and deallocation of physical pages without suffering from external fragmentation, the kernel employs the **Buddy System**:

```
Total Memory: [ 64 KB ]
Divided:      [ 32 KB ] [ 32 KB ]
Divided:      [ 16 KB ] [ 16 KB ] [ 32 KB ]
Allocated:    [ 8 KB* ] [ 8 KB ] [ 16 KB ] [ 32 KB ]
```
- Memory is grouped into power-of-two page blocks (order 0 = 4 KB, order 1 = 8 KB, ... order 10 = 4 MB).
- When a process requests memory, the allocator finds the smallest block that fits. If only larger blocks exist, it splits a block into two equal "buddies".
- When adjacent buddies are freed, the kernel instantly coalesces them back into a larger contiguous block!

---

## 2. Kernel Object Allocation: The SLAB / SLUB Allocator

The Buddy Allocator only works in chunks of whole 4 KB pages. But the kernel constantly creates tiny data structures that are only a few hundred bytes:
- `struct task_struct` (~6 KB)
- `struct inode` (~600 bytes)
- `struct dentry` (~192 bytes)
- `struct mm_struct` (~1 KB)

Using a full 4 KB page for every 200-byte inode would waste 95% of physical memory (**Internal Fragmentation**).

### The SLUB Allocator Solution:
The **SLUB allocator** takes 4 KB pages from the Buddy Allocator and carves them into dedicated, pre-initialized pools (caches) of fixed-size objects.

```bash
# Inspecting live kernel SLUB caches with slabtop:
$ sudo slabtop -s c | head -n 15
  OBJS ACTIVE  USE OBJ SIZE  SLABS OBJ/SLAB CACHE SIZE NAME
 98124  98000  99%    0.58K   3500       28     57400K inode_cache
142010 141800  99%    0.19K   3460       41     27680K dentry
  2410   2390  99%    4.12K    310        8     10240K task_struct
```

---

## 3. The Page Cache & The Dirty Page Lifecycle

The kernel minimizes disk I/O latency by caching file blocks in unused RAM via the **Page Cache**.

```
Application calls write()
         │
         ▼
Data written into RAM (Page Cache) ──► Marked as "Dirty"
         │
         ▼ (write() returns immediately to user space!)
[Background Kernel Daemon: kswapd / kworker flushes to disk after 30s]
         │
         ▼
Physical SSD / NVMe controller burns blocks
```

### Tuning Dirty Page Flushing via `/etc/sysctl.conf`:
- **`vm.dirty_background_ratio = 10`:** When dirty pages exceed 10% of total RAM, background threads begin flushing pages to disk.
- **`vm.dirty_ratio = 20`:** When dirty pages exceed 20% of total RAM, **all new writing processes are blocked** until dirty data is written to disk!

---

## 4. Memory Overcommit & The OOM Killer

By default, Linux permits **Memory Overcommit**: it allows programs to allocate more virtual memory than physically exists, assuming programs rarely use all allocated memory at once.

### Overcommit Settings (`vm.overcommit_memory`):
- **`0` (Heuristic Overcommit - Default):** Kernel uses heuristics to estimate if memory requests are reasonable.
- **`1` (Always Overcommit):** Kernel always grants memory requests; used heavily by Redis databases.
- **`2` (Never Overcommit):** Total memory allocations cannot exceed `Swap + (overcommit_ratio * RAM)`. Prevents OOM events entirely.

### The Out-Of-Memory (OOM) Killer Calculation:
When physical RAM and swap are completely exhausted:
1. The kernel calculates an integer score (`oom_score`) between `0` and `1000` for every active process.
2. The score is proportional to the percentage of RAM the process consumes.
3. The process with the highest score receives an unblockable **`SIGKILL` (signal 9)**.
4. Admins can protect critical services (like `sshd`) by adjusting `/proc/[PID]/oom_score_adj` to `-1000` (exempt from OOM).
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Process Management](./05-Process-Management.md) | [README](./README.md) | [07 - Filesystem and Storage](./07-Filesystem-and-Storage.md) |
