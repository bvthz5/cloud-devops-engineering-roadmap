# 05 — Historical System Activity Monitoring (sysstat & sar)

When a production incident occurred at 3:00 AM last night and resolved itself before you logged on, `sar` (System Activity Reporter) allows you to travel back in time and inspect exact CPU, memory, network, and disk metrics.

---

## 1. Enabling `sysstat` Background Collector

- **Debian / Ubuntu:**
  Edit `/etc/default/sysstat` and set `ENABLED="true"`.
  ```bash
  sudo systemctl enable --now sysstat
  ```
- **RHEL / Rocky Linux:**
  ```bash
  sudo systemctl enable --now sysstat
  ```
The daemon runs every 10 minutes via cron/systemd timer and writes binary historical data to `/var/log/sa/sa<day>`.

---

## 2. Time-Travel Queries with `sar`

```bash
# Query CPU utilization for today
sar -u

# Query CPU utilization between 03:00 AM and 04:00 AM specifically
sar -u -s 03:00:00 -e 04:00:00

# Query RAM and Swap memory utilization history
sar -r

# Query Disk I/O activity history
sar -d -p

# Query Network traffic history
sar -n DEV

# Query historical data from 2 days ago (e.g., 28th of the month)
sar -u -f /var/log/sa/sa28
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Disk IO Analysis and Storage Bottlenecks](./04-Disk-IO-Analysis-and-Storage-Bottlenecks.md) | [Index](../../../README.md) | [06 - Linux Kernel Tuning with sysctl →](./06-Linux-Kernel-Tuning-with-sysctl.md) |
