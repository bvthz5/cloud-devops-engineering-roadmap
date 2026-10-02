# 02 - Control Groups: cgroups v1 vs cgroups v2

## 1. Purpose of Control Groups (cgroups)

While **namespaces** control *what a process can see*, **control groups (cgroups)** control *how much hardware resources a process can consume* (CPU, Memory, Disk I/O, Network, PIDs).

---

## 2. Architectural Evolution: cgroups v1 vs cgroups v2

```text
cgroups v1 (Legacy Multi-Hierarchy):
/sys/fs/cgroup/
├── cpu/       ──► Process Group A
├── memory/    ──► Process Group B (Can differ from Group A!)
├── blkio/     ──► Process Group C
* Flaw: Controllers operated independently. Memory writeback could not correlate
  with block I/O throttling, causing unkillable OOM deadlocks.

cgroups v2 (Modern Unified Hierarchy):
/sys/fs/cgroup/
└── my_container/
    ├── cgroup.controllers (cpu, memory, io, pids)
    ├── cpu.max
    ├── memory.max
    ├── memory.current
    └── io.weight
* Benefit: A single unified process tree where memory, CPU, and I/O are tied to the exact same hierarchy!
```

---

## 3. Core cgroups v2 Controllers

### Memory Limits (`memory.max` and `memory.high`)
- **`memory.max`**: Hard limit. Exceeding this triggers the Linux kernel Out-Of-Memory (**OOM Killer**) to kill the process.
- **`memory.high`**: Throttle limit. When reached, the kernel throttles memory allocations and aggressively reclaims page cache rather than killing the process.

### CPU Limits (`cpu.max`)
- Syntax: `quota period` (e.g., `50000 100000` = 50ms of CPU every 100ms = 0.5 CPU core).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Linux Namespaces](./01-Linux-Kernel-Namespaces-Deep-Dive.md) | [README](./README.md) | [03 - Filesystem Isolation](./03-Filesystem-Isolation-chroot-to-pivot-root.md) |
