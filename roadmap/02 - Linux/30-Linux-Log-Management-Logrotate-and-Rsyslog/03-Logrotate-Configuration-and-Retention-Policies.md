# 03 — Logrotate Deep Dive and Retention Policies

`logrotate` is an automated utility that compresses, renames, and purges old log files according to administrative policies. It runs daily as a systemd timer or cron job (`/etc/cron.daily/logrotate`).

---

## 1. Production Configuration File Example

Create `/etc/logrotate.d/nginx`:

```text
/var/log/nginx/*.log {
    daily
    missingok
    rotate 14
    compress
    delaycompress
    notifempty
    create 0640 www-data adm
    sharedscripts
    postrotate
        [ -f /var/run/nginx.pid ] && kill -USR1 $(cat /var/run/nginx.pid)
    endscript
}
```

---

## 2. Core Directives Explained

- **`daily` / `weekly` / `monthly`:** Rotation interval.
- **`size 100M`:** Rotates immediately if the log exceeds 100MB, regardless of date.
- **`rotate 14`:** Retains 14 rotated archives before deleting the oldest.
- **`compress`:** Compresses rotated files using `gzip` (saves 80–90% disk space).
- **`delaycompress`:** Defers compression of the most recently rotated file (`.1`) until the next cycle, ensuring active processes can finish writing cleanly.
- **`missingok`:** Does not report an error if the log file is missing.
- **`notifempty`:** Skips rotation if the log file is 0 bytes.
- **`create 0640 user group`:** Immediately creates an empty log file with specified permissions.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - rsyslog Configuration and Remote Forwarding](./02-rsyslog-Configuration-and-Remote-Forwarding.md) | [Index](../../../README.md) | [04 - copytruncate vs create Signals →](./04-copytruncate-vs-create-Signals.md) |
