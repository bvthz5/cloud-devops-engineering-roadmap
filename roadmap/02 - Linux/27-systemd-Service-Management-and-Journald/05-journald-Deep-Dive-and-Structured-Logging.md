# 05 — journald Deep Dive and Structured Logging

`systemd-journald` is an operating system service that collects and stores logging data. It captures `stdout` and `stderr` from all systemd services, kernel ring buffer logs (`kmsg`), audit logs, and syslog messages in an indexed, binary format.

---

## 1. Fast Log Querying with `journalctl`

```bash
# Follow live logs for a specific service (like tail -f)
journalctl -u api-gateway.service -f

# Filter logs by time window
journalctl -u api-gateway.service --since "1 hour ago"
journalctl -u api-gateway.service --since "2026-10-01 10:00:00" --until "2026-10-01 11:30:00"

# Filter by log severity priority (0=emerg, 1=alert, 2=crit, 3=err, 4=warning, 5=notice, 6=info, 7=debug)
journalctl -p err..emerg

# View logs for the current boot only (critical for debugging post-reboot issues)
journalctl -b

# View logs for a specific Process ID
journalctl _PID=1420

# Output structured logs as JSON for ingestion into Datadog, Loki, or Elasticsearch
journalctl -u api-gateway.service -o json-pretty -n 5
```

---

## 2. Managing Log Storage & Retention

Configuration file: `/etc/systemd/journald.conf`

```ini
[Journal]
# Store logs permanently on disk in /var/log/journal/
Storage=persistent

# Limit maximum disk space used by journald
SystemMaxUse=2G

# Retain at least 500MB free disk space for other services
SystemKeepFree=500M

# Compress log files larger than 512 bytes
Compress=yes

# Maximum time to store journal entries
MaxRetentionSec=1month
```

Apply changes:
```bash
sudo systemctl restart systemd-journald

# Check how much disk space logs currently occupy
journalctl --disk-usage

# Manually vacuum old logs to free up space immediately
sudo journalctl --vacuum-size=1G
sudo journalctl --vacuum-time=7d
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - systemd Timers](./04-systemd-Timers-The-Modern-Cron-Replacement.md) | [README](./README.md) | [06 - cgroups v2 Resource Limits](./06-cgroups-v2-Resource-Limits-in-systemd.md) |
