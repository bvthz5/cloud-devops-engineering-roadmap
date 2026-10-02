# 03 — Memory Tuning: Swap, PageCache, and OOM

Linux aggressively utilizes unused RAM for **PageCache** (caching disk reads and writes). A Linux server with "only 100MB free RAM" is usually completely healthy because the kernel will instantly reclaim PageCache when applications request memory.

---

## 1. Reading Memory with `free -h`

```text
               total        used        free      shared  buff/cache   available
Mem:            31Gi        18Gi       1.2Gi       512Mi        12Gi        12Gi
Swap:          8.0Gi       256Mi       7.7Gi
```
- **`used`:** Memory actively allocated by application processes (anonymous memory).
- **`buff/cache`:** Memory used by kernel to cache disk blocks and files.
- **`available`:** **The most important metric.** The amount of memory available to start new applications without swapping (Free + reclaimable PageCache).

---

## 2. Linux Swappiness (`vm.swappiness`)

`vm.swappiness` (range 0 to 100 or 200 in modern kernels) controls the kernel's preference to reclaim anonymous memory to swap vs reclaiming PageCache:
- `vm.swappiness = 60` (Default): Balanced.
- `vm.swappiness = 10` or `1`: Recommended for high-performance databases (PostgreSQL, MySQL, Redis, Elasticsearch). Prevents paging active application memory to slow disk while maintaining emergency swap headroom.

```bash
# Check current swappiness
cat /proc/sys/vm/swappiness

# Set swappiness permanently
echo "vm.swappiness = 10" | sudo tee /etc/sysctl.d/99-swappiness.conf
sudo sysctl -p /etc/sysctl.d/99-swappiness.conf
```

---

## 3. Investigating OOM Killer Invocations

When RAM + Swap is 100% exhausted, the kernel Out-Of-Memory (OOM) killer selects a sacrificial process based on its `badness` score and terminates it with `SIGKILL`.

```bash
# Check if OOM killer has triggered
sudo dmesg -T | grep -i -E 'killed process|oom_reaper'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - CPU Profiling](./02-CPU-Profiling-and-Bottleneck-Analysis.md) | [README](./README.md) | [04 - Disk I/O Analysis](./04-Disk-IO-Analysis-and-Storage-Bottlenecks.md) |
