# 17 — Cron Jobs and Systemd Timers: Setup and Fixes

## 1. Crontab Syntax & Environment Fixes
```
* * * * * command
```
- PATH must be declared in crontab header.
- Redirect stdout and stderr (`>> /var/log/cron.log 2>&1`).

## 2. Systemd Timers Setup
- `.service` unit file handles execution command.
- `.timer` unit file handles schedule (`OnCalendar=*-*-* 02:00:00`).
