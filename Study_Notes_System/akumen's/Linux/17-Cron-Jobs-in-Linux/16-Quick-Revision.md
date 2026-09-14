# 16. Quick Revision

## Core idea
**Cron** = Time-based Linux job scheduler.

## Commands
- `crontab -e` : Edit crontab
- `crontab -l` : List crontab entries
- `crontab -r` : Remove all jobs (Dangerous!)
- `sudo crontab -u alice -e` : Edit Alice's crontab

## Five Fields
`MINUTE HOUR DAY_OF_MONTH MONTH DAY_OF_WEEK COMMAND`

## Field Ranges
- **Minute:** `0-59`
- **Hour:** `0-23`
- **Day of month:** `1-31`
- **Month:** `1-12`
- **Day of week:** `0-6` (Sunday=0 or 7)

## Symbols
- `*` : Every interval
- `,` : List separator (`1,15`)
- `-` : Range (`1-5`)
- `/` : Step interval (`*/5`)

## Common Expressions
- `* * * * *` : Every minute
- `0 * * * *` : Every hour
- `*/5 * * * *` : Every 5 minutes
- `0 2 * * *` : Daily at 2:00 AM
- `0 9 * * 1-5` : Weekdays at 9:00 AM
- `0 0 * * 0` : Sunday midnight

## Shortcuts
- `@reboot` : Run once at boot
- `@hourly` : `0 * * * *`
- `@daily` : `0 0 * * *`
- `@weekly` : `0 0 * * 0`
- `@monthly` : `0 0 1 * *`
- `@yearly` : `0 0 1 1 *`

## Redirection
- `>> /path/job.log 2>&1` : Append stdout and stderr to log file

## Service Checks
- `systemctl status cron` (Debian/Ubuntu)
- `systemctl status crond` (RHEL/CentOS)

## Log Checks
- `grep CRON /var/log/syslog`
- `sudo tail -f /var/log/cron`
- `journalctl | grep CRON`

## Common Causes of Failure
- Wrong schedule syntax
- Service stopped
- Relative paths used instead of absolute paths
- Missing `PATH` or environment variables
- Script missing executable permissions (`chmod +x`)
- Working directory mismatches

## Security & Best Practices
- Use least privilege (avoid root where possible).
- Use absolute paths everywhere.
- Secure script permissions & parent directories.
- Avoid plain-text passwords in crontabs.
- Log important jobs and rotate log files.
- Prevent overlapping runs using `flock`.

## Alternatives
- **cron** → Simple recurring tasks.
- **anacron** → Periodic tasks on systems with downtime (laptops).
- **systemd timer** → Linux service integration & dependency management.
- **Kubernetes CronJob** → Containerized workloads.
