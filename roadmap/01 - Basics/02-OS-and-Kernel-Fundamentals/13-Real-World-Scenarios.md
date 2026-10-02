# 13 — Real-World OS & Kernel Scenarios for SREs

Production engineering is where theoretical kernel concepts intersect with critical uptime outages. Below are five real-world enterprise outage scenarios driven by kernel mechanisms.

---

## Scenario 1: Kubernetes Pod CPU Throttling Under CFS Quota

### Problem Statement
A critical Java payments microservice running in Kubernetes experiences severe P99 latency degradation (jumping from 15ms to 1,200ms) during traffic bursts. CPU utilization metrics in Grafana show the pod is consuming only 1.2 cores out of its 2-core `limits.cpu`.

### Architecture Analysis
Kubernetes enforces CPU limits using the Linux kernel **Completely Fair Scheduler (CFS) bandwidth control** (`cpu.cfs_quota_us` and `cpu.cfs_period_us` in cgroups).
- The default period is 100ms (`100,000 µs`).
- A 2-core limit grants `200,000 µs` of CPU execution time per 100ms window.
- Because Java is multi-threaded, if 20 threads wake up simultaneously to handle an incoming batch of requests, each running for just 10ms, they consume `20 * 10ms = 200ms` of CPU time within the first **15ms of the period**!
- The kernel CFS scheduler immediately throttles the entire container for the remaining **85ms** of the quota window.

```text
100ms CFS Period Window
[ 20 Threads consume 200ms quota ] ────► [ KERNEL FORCIBLY THROTTLES CONTAINER ]
|<────────── 15ms ───────────────>|      |<────────────── 85ms ────────────────>|
                                          Incoming HTTP requests queue up & stall!
```

### Verification
```bash
# Check CFS throttling metrics inside the container cgroup
cat /sys/fs/cgroup/cpu/cpu.stat
# nr_periods: 45100
# nr_throttled: 12450  <-- High throttled count confirms issue!
# throttled_time: 85210492100 ns
```

### Engineering Solution
1. **Remove CPU Limits on Latency-Sensitive Pods:**
   Industry best practice (pioneered by Buffer, Zalando, and Google) is to set high CPU **requests** but omit CPU **limits** for latency-sensitive microservices, avoiding artificial CFS throttling while still preventing noisy neighbor contention:
   ```yaml
   resources:
     requests:
       cpu: "2"
       memory: "4Gi"
     limits:
       memory: "4Gi" # Keep memory limit to protect host from leaks
   ```
2. **Outcome:**
   CFS throttling dropped to 0%; P99 latency returned to a consistent 14ms flat.

---

## Scenario 2: Node PID Exhaustion via Docker Zombie Leaks

### Problem Statement
An entire Kubernetes worker node with 128 GB of RAM and 32 CPU cores stops accepting new pods with error:
`failed to create pod sandbox: fork/exec /usr/bin/runc: cannot allocate memory`.
However, `free -m` reports 90 GB of RAM free!

### Architecture Analysis
The error string `cannot allocate memory` during a `fork()` syscall is misleading. Linux returns `-ENOMEM` from `fork()` when **the kernel process table has exhausted all available Process IDs (`pid_max`)**.
- A custom batch worker container was spawning child CLI processes without calling `wait()`.
- Over 48 hours, 32,760 **Zombie (`<defunct>`)** processes accumulated on the host.
- The host hit `/proc/sys/kernel/pid_max` (`32768`).
- Result: No process on the entire node (including root SSH sessions) could spawn child commands.

### Diagnostic Command
```bash
# Count total running processes and zombies
ps aux | awk '{print $8}' | sort | uniq -c
# Output:
#     14 S
#     22 R
#  32732 Z  <-- 32,732 Zombies!
```

### Engineering Solution
1. **Immediate Mitigation:** Kill the broken parent container to allow PID 1 to reap the zombies:
   ```bash
   docker kill <container_id>
   ```
2. **Permanent Fix:**
   - Add an init process (`--init` or `tini`) to the container image entrypoint so child processes are reaped automatically.
   - Configure Kubernetes cgroups PID limit (`podPidsLimit: 4096`) in Kubelet config to prevent a single buggy pod from exhausting the host's PID pool.
   - Increase host PID ceiling in `/etc/sysctl.d/99-pids.conf`:
     ```ini
     kernel.pid_max = 4194304
     ```

---

## Scenario 3: Nginx "Too Many Open Files" 502 Gateway Outage

### Problem Statement
During a major product launch, an Nginx reverse proxy starts dropping 40% of inbound connections with HTTP 502 Bad Gateway and logging:
`2026/10/02 07:30:12 [alert] 1042#1042: *45120 socket() failed (24: Too many open files)`.

### Architecture Analysis
Every incoming TCP connection consumes one file descriptor. Every reverse-proxy upstream connection to a backend server consumes a second file descriptor.
- 10,000 concurrent client connections require at least 20,000 open file descriptors, plus logs and static files.
- While `/proc/sys/fs/file-max` was set to 2,000,000, the **systemd service unit** for Nginx defaulted to a process soft limit of `1024`.

### Engineering Solution
1. **Update systemd Service Limit:**
   ```bash
   sudo mkdir -p /etc/systemd/system/nginx.service.d/
   cat <<EOF | sudo tee /etc/systemd/system/nginx.service.d/limits.conf
   [Service]
   LimitNOFILE=1048576
   EOF
   ```
2. **Update Nginx Worker Configuration (`nginx.conf`):**
   ```nginx
   events {
       worker_connections 65535;
   }
   worker_rlimit_nofile 1048576;
   ```
3. **Apply Changes with Zero Downtime:**
   ```bash
   sudo systemctl daemon-reload
   sudo nginx -s reload
   ```

---

## Scenario 4: Redis Latency Spike Caused by `vm.overcommit_memory`

### Problem Statement
A Redis caching cluster experiences 5-second query timeouts whenever its background snapshotting job (`BGSAVE`) runs.

### Architecture Analysis
Redis persists its in-memory dataset to disk by calling `fork()`.
- The child process writes the database to a `.rdb` file while the parent continues servicing client queries using **Copy-On-Write (COW)**.
- If `/proc/sys/vm/overcommit_memory` is set to `0` (Heuristic Overcommit), the kernel calculates that Redis needs double its memory allocation to fork safely.
- If physical RAM is tight, the kernel delays or rejects the fork, or swaps existing pages to disk during COW writes, resulting in multi-second stalls.

### Engineering Solution
Configure the kernel to allow unconditional memory overcommit:
```bash
sudo sysctl -w vm.overcommit_memory=1
echo "vm.overcommit_memory = 1" | sudo tee -a /etc/sysctl.d/99-redis.conf
```
Redis can now `fork()` instantaneously without waiting for memory verification.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 12 - Modern Kernel Tech cgroups v2 eBPF and seccomp](./12-Modern-Kernel-Tech-cgroups-v2-eBPF-and-seccomp.md) | [Index](../../../README.md) | [14 - Troubleshooting →](./14-Troubleshooting.md) |
