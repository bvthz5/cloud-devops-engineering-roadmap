# 10 — Hardware & Architecture Troubleshooting Guide

This guide provides systematic, production-tested troubleshooting procedures for hardware-related bottlenecks, hypervisor resource starvation, memory bus congestion, and architecture mismatches.

---

## 1. Diagnostic Decision Tree

```text
                       [ High Latency / Slow System ]
                                     |
              +----------------------+----------------------+
              |                                             |
       [ High CPU Usage ]                           [ Low CPU Usage ]
              |                                             |
    +---------+---------+                         +---------+---------+
    |                   |                         |                   |
[%usr / %sys high]   [%st high]                [High %wa]          [Memory High]
    |                   |                         |                   |
Runaway Process     Cloud Hypervisor        Disk I/O Wait       OOM Killer / Swap
Check `pidstat 1`   Throttling / Steal      Check `iostat -xz`  Check `free -m`
Check Thermal Clock Switch to Dedicated     EBS/NVMe saturated  Check `vmstat 1`
```

---

## 2. Issue 1: High CPU Steal Time (%st) on Cloud Virtual Machines

### Symptoms
- System latency escalates dramatically.
- `top` or `htop` displays CPU load high, but user application CPU percentage (`%usr`) is low.
- `vmstat 1` shows double-digit numbers in the `st` column.

### Root Cause
The cloud provider's underlying physical hardware host (hypervisor) is oversubscribed, or the virtual instance is a burstable type (AWS `t2`/`t3`, Azure `B-series`) that has consumed all its CPU credits.

### Step-by-Step Diagnostic Commands
```bash
# 1. Inspect live CPU state breakdown
top -b -n 1 | head -n 10

# 2. Monitor CPU steal over 5-second intervals
mpstat -P ALL 1 5

# 3. Check AWS EC2 CPU credit balance (if AWS CLI is installed on node)
aws cloudwatch get-metric-data \
  --metric-data-queries file://cpu-credits-query.json \
  --start-time $(date -u -d '1 hour ago' +%FT%TZ) \
  --end-time $(date -u +%FT%TZ)
```

### Remediation
1. For burstable instances: Enable unlimited mode (`aws ec2 modify-instance-credit-specification`) or immediately resize to a compute-optimized instance class (`c6i.large`, `c7g.large`).
2. For dedicated instances: Contact cloud provider support; the underlying physical hardware hypervisor node is experiencing noisy neighbor contention or failing hardware.

---

## 3. Issue 2: Storage I/O Wait Bottlenecks (`%wa`) & High Queue Depth

### Symptoms
- CPU utilization appears low, but total system load average (`uptime`) is high (e.g., Load 16 on a 4-core machine).
- Applications hang on database queries or file writes.
- `top` shows high `%wa` (I/O Wait).

### Diagnostic Procedure
```bash
# 1. Identify which disk device is saturated
iostat -xz 1 5

# Sample Output:
# Device    r/s    w/s   rMB/s   wMB/s  aqu-sz  await  %util
# nvme0n1  12.0  850.0    0.50   45.20   32.40  38.12  98.50
```
- **`aqu-sz` (Average Queue Size):** If significantly higher than 1-2 per queue, requests are backing up.
- **`await` (Average Service Time):** If above 10-15ms on SSD/NVMe, storage is severely degraded.
- **`%util`:** If approaching 100%, the storage device cannot handle more concurrent I/O operations.

```bash
# 2. Identify which process is generating the I/O
iotop -oPa -d 2
# Or using pidstat:
pidstat -d 1 5
```

### Remediation
1. Adjust I/O scheduling algorithm if on bare-metal (`kyber` or `none` for NVMe SSDs):
   ```bash
   echo none > /sys/block/nvme0n1/queue/scheduler
   ```
2. Increase EBS IOPS / Throughput or migrate data to local NVMe instances.
3. Optimize application write buffers (e.g., enable write batching in Elasticsearch, PostgreSQL `wal_buffers`).

---

## 4. Issue 3: Memory Exhaustion, OOM Killer, and Swap Thrashing

### Symptoms
- Critical daemons (`mysqld`, `java`, `node`) abruptly disappear without clean shutdown logs.
- Memory usage jumps to 99%, and swap space usage spikes.
- `dmesg` reports: `Out of memory: Killed process 4128 (java) total-vm:8542124kB, anon-rss:3910240kB`.

### Root Cause
The operating system ran out of physical memory (DRAM). Linux triggers the **Out-Of-Memory (OOM) Killer**, calculating a badness score for every process and terminating the highest-scoring candidate to protect kernel stability.

### Diagnostic Procedure
```bash
# 1. Check current memory, buffer, and cache allocations
free -h

# 2. View swap activity (si = swap-in, so = swap-out)
vmstat 1 5
# High 'so' numbers indicate active swap thrashing (extreme slowdown)

# 3. Check kernel dmesg for OOM killer invocations
dmesg -T | grep -E -i "oom[-_]killer|killed process"

# 4. Inspect top memory-consuming processes
ps aux --sort=-%mem | head -n 10
```

### Remediation
1. Adjust process `oom_score_adj` to protect critical system services from being terminated:
   ```bash
   # Make sshd immune to OOM killer (-1000)
   echo -1000 > /proc/$(pgrep sshd | head -1)/oom_score_adj
   ```
2. In Kubernetes, set explicit memory limits and requests in Pod specs:
   ```yaml
   resources:
     requests:
       memory: "2Gi"
     limits:
       memory: "4Gi"
   ```
3. Tune swappiness if swap usage degrades performance:
   ```bash
   sysctl -w vm.swappiness=10
   ```

---

## 5. Issue 4: Interrupt Storms & High Software Interrupts (`%si`)

### Symptoms
- System latency spikes during heavy network ingress (e.g., 10 Gbps DDoS or packet ingestion).
- `top` indicates high `%si` (Software Interrupts) on one CPU core while others are idle.
- Packet drops occur at the NIC level even though overall CPU usage is under 30%.

### Root Cause
Network interface cards generate interrupts when receiving packets. If **Receive Side Scaling (RSS)** or SMP IRQ affinity is misconfigured, all hardware interrupt requests (IRQs) are funneled to CPU Core 0, overwhelming its L1 cache and execution pipeline.

### Diagnostic Procedure
```bash
# 1. Check interrupt distribution across CPU cores
cat /proc/interrupts | grep -E "eth0|ens|nvme"

# 2. Inspect per-core CPU breakdown
mpstat -P ALL 1 3
```

### Remediation
1. Enable and configure `irqbalance` daemon:
   ```bash
   systemctl enable --now irqbalance
   ```
2. Manually distribute network ring buffers across CPU cores using `ethtool`:
   ```bash
   ethtool -L eth0 combined 8
   ```

---

## 6. Issue 5: Missing Hardware Virtualization (VT-x / AMD-V)

### Symptoms
- KVM, Docker Desktop, VirtualBox, or Minikube fails to start with error:
  `ERROR: KVM acceleration can not be used (/dev/kvm: No such file or directory)`.

### Diagnostic Procedure
```bash
# Check if CPU exposes virtualization extensions
egrep -c '(vmx|svm)' /proc/cpuinfo
# 0 = Virtualization disabled or unsupported
# >= 1 = Virtualization supported and enabled

# Check if KVM kernel modules are loaded
lsmod | grep kvm
```

### Remediation
1. On bare-metal servers: Enter UEFI/BIOS setup (F2/Del) and enable **Intel Virtualization Technology (VT-x)** or **AMD-V (SVM)**.
2. In Cloud VMs (Nested Virtualization):
   - Azure: Choose v3 or v4 instances (e.g., `Standard_D4s_v3`).
   - GCP: Add `--enable-nested-virtualization` flag during instance creation.
   - AWS: Use bare metal instances (`c5.metal`, `c6i.metal`) or instances supporting nested virtualization.

---

## 7. Issue 6: CPU Thermal Throttling & Frequency Capping

### Symptoms
- Compute-heavy jobs take 3x longer to finish than expected.
- Server fan speeds run at 100% RPM.
- Cloud bare-metal instances experience sudden, unexplainable throughput drops.

### Diagnostic Procedure
```bash
# 1. Check current CPU operating frequency across all cores
lscpu | grep -i "mhz"
# Or:
cat /proc/cpuinfo | grep "MHz"

# 2. Check if CPU is being actively throttled
cat /sys/devices/system/cpu/cpu*/thermal_throttle/package_throttle_count

# 3. Check hardware sensors (temperatures)
sensors
```

### Remediation
1. Inspect server chassis airflow and clean dust buildup in data centers.
2. Verify thermal paste integrity between CPU heat spreader and cooling block.
3. In cloud bare-metal environments, alert infrastructure provider to migrate workloads off the failing blade.

---

## Hardware Troubleshooting Command Reference Cheat Sheet

| Symptom | Primary Tool | Key Metric / Threshold | Corrective Action |
| :--- | :--- | :--- | :--- |
| **High CPU Steal** | `mpstat 1` | `%st > 5%` | Switch instance from burstable to dedicated |
| **I/O Bottleneck** | `iostat -xz 1` | `%util > 85%`, `await > 15ms` | Upgrade IOPS, switch scheduler to `none`/`kyber` |
| **OOM Killing** | `dmesg -T` | `Out of memory` logged | Add RAM, tune `oom_score_adj`, enforce Pod limits |
| **IRQ Core Saturation**| `/proc/interrupts` | Single core handling all NIC IRQs | Start `irqbalance`, tune RSS queues with `ethtool` |
| **Virtualization Error**| `egrep '(vmx\|svm)'`| Output `0` | Enable VT-x/SVM in BIOS or enable nested virt |
| **Thermal Throttling**| `sensors` / `lscpu` | Clock speed dropped below base GHz | Check cooling, replace thermal interface, re-seat fans |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Real World Scenarios](./09-Real-World-Scenarios.md) | [Index](../../../README.md) | [11 - Interview QA →](./11-Interview-QA.md) |
