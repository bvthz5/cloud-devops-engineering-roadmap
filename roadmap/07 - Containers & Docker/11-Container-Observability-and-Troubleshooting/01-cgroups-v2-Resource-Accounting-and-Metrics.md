# 01 - cgroups v2 Resource Accounting and Metrics

## 1. Inspecting Live cgroups v2 Telemetry

In cgroups v2, all resource telemetry for a container is exposed directly as plaintext files in `/sys/fs/cgroup/`:

```bash
# 1. Real-time memory consumption (bytes)
cat /sys/fs/cgroup/system.slice/docker-<id>.scope/memory.current

# 2. CPU quota and throttling statistics
cat /sys/fs/cgroup/system.slice/docker-<id>.scope/cpu.stat
# Output:
# usage_usec 4512398
# user_usec 3120194
# system_usec 1392204
# nr_periods 1000       <-- Total scheduling periods
# nr_throttled 250      <-- Throttled periods (25% CPU throttling!)
# throttled_usec 129034
```

---

## 2. Pressure Stall Information (PSI)

PSI measures how much time a container spends starved for hardware:
- **`memory.pressure`**: Tracks time tasks stalled waiting for memory paging/reclaim.
- **`cpu.pressure`**: Tracks time runnable tasks spent waiting for an available CPU core.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Container Exit Codes & Forensics](./02-Container-Exit-Codes-and-Crash-Forensics.md) |
