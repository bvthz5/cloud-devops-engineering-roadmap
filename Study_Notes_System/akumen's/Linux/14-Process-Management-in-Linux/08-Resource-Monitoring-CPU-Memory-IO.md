# 08 - Resource Monitoring (CPU, Memory, I/O)

Process management isn't just about killing processes; it's about understanding how they consume system resources. When a system degrades, you must isolate the bottleneck: is it CPU, Memory, or Disk I/O?

---

## 🧠 Monitoring Memory (`free`)

The `free` command displays the total amount of free and used physical memory and swap space.

```bash
free -h
```
*The `-h` flag makes it human-readable (MB, GB).*

**Understanding the Output:**
```text
              total        used        free      shared  buff/cache   available
Mem:           15Gi       4.5Gi       2.0Gi       1.1Gi       9.0Gi        10Gi
Swap:         4.0Gi       0.0Ki       4.0Gi
```

*   **used:** Memory explicitly claimed by processes.
*   **buff/cache:** Linux aggressively uses free RAM to cache disk reads/writes to speed up the system. This is a *good* thing.
*   **available:** The true amount of memory available for starting new applications. (It is roughly `free` + `buff/cache` that can be quickly freed). 
*   *Do not panic if `free` is low, as long as `available` is high.*

---

## ⏱️ Monitoring Load Average (`uptime` / `top`)

Load average is a metric of system stress. It represents the average number of processes in the run queue (`R` state) PLUS the number of processes waiting for disk/network I/O (`D` state).

```bash
uptime
# Output: 10:23:45 up 14 days,  2:34,  2 users,  load average: 1.50, 0.75, 0.30
```

The three numbers represent the load average over the last **1 minute**, **5 minutes**, and **15 minutes**.

**How to interpret Load Average:**
The "danger zone" depends on your CPU core count (run `nproc` to see).
*   **1.0 on a 1-core system:** The CPU is at exactly 100% capacity.
*   **4.0 on a 4-core system:** The CPUs are at exactly 100% capacity.
*   If load average > core count: Processes are waiting in line. The system is overloaded.
*   *Note: High load average with 0% CPU usage usually means a failing hard drive causing processes to pile up in the 'D' state.*

---

## 💾 Monitoring Disk I/O (`iostat` / `iotop`)

If CPU and Memory look fine, but the system is sluggish, the bottleneck is likely the storage drive.

**`iostat`:** Shows overall CPU and device I/O statistics. (Part of the `sysstat` package).
```bash
iostat -xz 1 3
```
*   `-x`: Extended stats (most useful).
*   `-z`: Omit devices with zero activity.
*   `1 3`: Refresh every 1 second, 3 times.
Look at the **`%util`** column. If a disk is consistently at 90-100%, your disk is saturated.

**`iotop`:** Like `top`, but for disk usage. Shows exactly which process is reading/writing the most. (Often requires `sudo iotop`).

---

## 📂 Monitoring Open Files (`lsof`)

In Linux, "everything is a file" (including network sockets). 

`lsof` (List Open Files) is an invaluable tool for tracing what a process is actually doing.

```bash
# See all files opened by the Nginx process
sudo lsof -c nginx

# See which process is using port 80 (HTTP)
sudo lsof -i :80

# See which process is holding a deleted log file open (causing disk space leaks)
sudo lsof | grep deleted
```
