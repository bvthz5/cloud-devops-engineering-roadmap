# 9. System-Wide Cron

## User crontab vs system cron

A user's crontab is normally edited with:
```bash
crontab -e
```

System-wide cron configuration is found in locations such as:
- `/etc/crontab`
- `/etc/cron.d/`
- `/etc/cron.hourly/`
- `/etc/cron.daily/`
- `/etc/cron.weekly/`
- `/etc/cron.monthly/`

## `/etc/crontab`

A key difference is that the system crontab includes an explicit **user** field.

Typical structure:
```cron
# minute hour day-of-month month day-of-week user command
0 2 * * * root /usr/local/bin/backup.sh
```
The user field tells cron which account should run the command.

## `/etc/cron.d/`

This is commonly used for package or system-managed cron definitions.

Files in this directory use the system-style format with a user field.

## Periodic Directories

Common locations:
- `/etc/cron.hourly/`
- `/etc/cron.daily/`
- `/etc/cron.weekly/`
- `/etc/cron.monthly/`

These are intended for scripts that run at the corresponding periodic frequency.

## Why system-wide cron exists

It is useful for:
- System administration
- Package-managed maintenance
- Jobs that must run as a particular service/system account
- Centralized machine-level scheduling

## Security warning

Do not place arbitrary scripts into system cron locations unless you understand:
- Who runs the script
- File ownership
- Permissions
- `PATH`/environment
- What the script does
- Whether untrusted users can modify it

A root-run cron script can become a serious security risk if an unprivileged user can modify the script or anything it executes.

## Source foundation

The supplied source identifies `/etc/crontab`, `/etc/cron.d/`, and the hourly/daily/weekly/monthly directories.
