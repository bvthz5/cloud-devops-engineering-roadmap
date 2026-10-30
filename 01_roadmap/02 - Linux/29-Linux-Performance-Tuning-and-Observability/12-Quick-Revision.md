# 12 — Quick Revision Cheat Sheet: Performance & Observability

---

## 1. Top Observability Commands

```bash
# CPU & Load
uptime                        # System load average
mpstat -P ALL 1               # Per-core CPU breakdown (%usr, %sys, %iowait)
pidstat 1                     # Process-level CPU consumption

# Memory
free -h                       # Memory & PageCache overview
vmstat 1                      # Run queue, memory, and swap metrics

# Disk I/O
iostat -xz 1                  # Device %util, await latency, IOPS
sudo iotop -oPa               # Per-process disk I/O bandwidth

# History
sar -u                        # Historical CPU utilization
sar -r                        # Historical memory utilization
sar -d -p                     # Historical disk utilization
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (30-Linux-Log-Management-Logrotate-and-Rsyslog) →](../30-Linux-Log-Management-Logrotate-and-Rsyslog/01-Linux-Logging-Architecture-and-var-log.md) |
