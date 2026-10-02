# 1. What Is Cron?

## Simple definition

**Cron** is a time-based scheduler built into Linux. It runs commands or scripts automatically at specified times.

A **cron job** is one scheduled task.

Examples:
- Run a backup every night at 2 AM.
- Clean temporary files every Sunday.
- Generate a report every weekday morning.
- Sync data every 15 minutes.

## Why cron is useful

Without a scheduler:

```text
Administrator
     |
     +--> remembers task
     |
     +--> logs in
     |
     +--> runs command manually
     |
     +--> repeats tomorrow
```

With cron:

```text
                 +----------------+
Time reaches --> | Cron scheduler |
                 +-------+--------+
                         |
                         v
                  Run command/script
                         |
                         v
                  Save output/log
```

Cron is useful for repetitive, predictable tasks.

## Important idea

Cron does not magically understand your application.

It simply checks the schedule and starts the configured command.

The command or script is responsible for doing the actual work.

## Common DevOps uses
- Backups
- Log cleanup
- Temporary-file cleanup
- Report generation
- Database maintenance
- Certificate checks
- Synchronization
- Health checks
- Scheduled scripts
- Periodic housekeeping

## Cron vs a normal command

Normal command:
```bash
./backup.sh
```
You run it now.

Cron job:
```cron
0 2 * * * /home/alice/scripts/backup.sh
```
Cron runs it according to the schedule.

## Key terms
| Term | Meaning |
| --- | --- |
| `cron` | Scheduler/service mechanism |
| `cron job` | One scheduled task |
| `crontab` | Table containing scheduled jobs |
| `crontab -e` | Edit jobs |
| `crontab -l` | List jobs |
| `crond` / `cron` | Cron service/process name, depending on distro |

## Source foundation

The supplied source defines cron as a time-based Linux scheduler and gives backup, cleanup, reporting, and synchronization as examples.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Crontab and Commands](./02-Crontab-and-Commands.md) |
