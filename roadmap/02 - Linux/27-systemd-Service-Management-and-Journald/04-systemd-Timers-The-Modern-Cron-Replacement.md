# 04 — systemd Timers: The Modern Cron Replacement

`systemd` timers replace legacy `cron` jobs with superior observability, microsecond precision, automatic missed-job recovery, and dependency chaining.

---

## 1. Why systemd Timers Beat `cron`

| Feature | Legacy `cron` | Modern `systemd` Timers |
| :--- | :--- | :--- |
| **Execution Logging** | Silent failure; buried in syslog or mailed | Integrated into `journalctl` with execution duration |
| **Failure Handling** | Overlapping jobs stack up and crash system | Prevents overlap; monitors exit status and triggers alert units |
| **Calendar Syntax** | Cryptic 5-field syntax (`* * * * *`) | Human-readable `OnCalendar=Mon..Fri *-*-* 03:00:00` |
| **Missed Executions**| If server was off, job is lost forever | `Persistent=true`: executes missed jobs immediately upon boot |
| **Resource Limits** | Inherits ambient shell environment | Governed by service unit cgroups (`MemoryMax`, `CPUQuota`) |

---

## 2. Creating a Timer Pair

A timer always consists of two files with matching names:
1. A `.service` file defining **WHAT** to run.
2. A `.timer` file defining **WHEN** to run it.

### Step 1: Create the Worker Service (`/etc/systemd/system/db-backup.service`)
```ini
[Unit]
Description=PostgreSQL Nightly Database Backup
After=network-online.target

[Service]
Type=oneshot
User=postgres
ExecStart=/usr/local/bin/pg-backup.sh
StandardOutput=journal
StandardError=journal
```

### Step 2: Create the Timer Unit (`/etc/systemd/system/db-backup.timer`)
```ini
[Unit]
Description=Run Database Backup Every Night at 2:00 AM
Requires=db-backup.service

[Timer]
# Run every day at 02:00 AM UTC
OnCalendar=*-*-* 02:00:00

# Catch up if system was powered down or sleeping at scheduled time
Persistent=true

# Add randomized jitter up to 15 minutes to prevent hammering shared storage
RandomizedDelaySec=15m

[Install]
WantedBy=timers.target
```

### Step 3: Enable and Inspect
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now db-backup.timer

# View all active system timers and next trigger time
systemctl list-timers
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Hardening & Sandboxing](./03-Hardening-and-Sandboxing-Services.md) | [README](./README.md) | [05 - journald Structured Logging](./05-journald-Deep-Dive-and-Structured-Logging.md) |
