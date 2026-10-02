# 06 — cgroups v2 Resource Limits in systemd

Control Groups (cgroups) are a Linux kernel mechanism that organizes processes hierarchically and distributes system resources (CPU, Memory, I/O, PIDs) along the hierarchy. `systemd` is the primary user-space manager for cgroups v2.

---

## 1. systemd Slices

systemd divides the system into hierarchical resource partitions called **Slices**:
- `-.slice`: Root slice.
- `system.slice`: Default slice for all system services and background daemons.
- `user.slice`: Default slice for interactive logged-in user sessions.

---

## 2. Configuring Resource Limits in `.service` Units

```ini
[Service]
# Limit CPU usage to 200% (2 CPU cores on multi-core machine)
CPUQuota=200%

# Memory Throttle Threshold: Kernel starts aggressively reclaiming page cache
MemoryHigh=1.5G

# Hard Memory Limit: If breached, process is killed immediately by Linux OOM-killer
MemoryMax=2G

# Limit maximum number of processes/threads (prevents fork bombs!)
TasksMax=1024

# Disk I/O Bandwidth limit
IOReadBandwidthMax=/dev/sda 50M
IOWriteBandwidthMax=/dev/sda 20M
```

---

## 3. Real-Time Resource Monitoring

```bash
# Live top-like monitor showing CPU, Memory, and I/O grouped by systemd unit
systemd-cgtop

# Inspect current cgroup properties of a service
systemctl show api-gateway.service -p MemoryCurrent,CPUUsageNSec,TasksCurrent

# Dynamically set a temporary memory limit on a running service without restart!
sudo systemctl set-property api-gateway.service MemoryMax=1G
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - journald Structured Logging](./05-journald-Deep-Dive-and-Structured-Logging.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
