# 11 - Job Scheduling: cron and at

Sometimes you need a process to start automatically, but not continuously like a systemd daemon. You need it to run at a specific time or on a recurring schedule (e.g., daily backups, log rotation).

---

## ⏰ 1. One-Time Execution: `at`

If you need to run a script exactly once in the future, use the `at` command. (You may need to `apt install at`).

### Usage
```bash
# Schedule for a specific time today
at 23:00

# Schedule for a relative time
at now + 2 hours

# Schedule for a specific date
at 9:00 AM July 25
```
Once you enter the command, you will be dropped into an interactive prompt (`at>`). Type the commands you want to run, and press `Ctrl+D` to save and exit.

**Viewing and Managing `at` jobs:**
*   `atq`: List pending jobs.
*   `atrm [job_number]`: Cancel a pending job.

---

## 🔄 2. Recurring Execution: `cron`

`cron` is the standard Linux daemon for executing scheduled commands (cron jobs).

### The Crontab
Each user has their own cron configuration file, edited using the `crontab` command.
```bash
# Edit your personal crontab (usually opens in nano or vi)
crontab -e

# List the contents of your crontab
crontab -l
```

### The Cron Syntax
A crontab entry consists of 5 time-and-date fields followed by the command to execute.

```text
* * * * * command_to_execute
- - - - -
| | | | |
| | | | +----- Day of the week (0 - 7) (Sunday=0 or 7)
| | | +------- Month (1 - 12)
| | +--------- Day of the month (1 - 31)
| +----------- Hour (0 - 23)
+------------- Minute (0 - 59)
```

**Examples:**
```text
# Run a backup script every day at 2:30 AM
30 2 * * * /opt/scripts/backup.sh

# Run a health check every 5 minutes
*/5 * * * * /opt/scripts/health.sh

# Run a report at 5:00 PM every Friday
0 17 * * 5 /opt/scripts/weekly_report.sh
```

### System-Wide Cron
There is also a system-wide crontab (`/etc/crontab`). It has an extra field specifying the user to run the command as:
```text
# m h dom mon dow user  command
30 2 * * * root /opt/scripts/system_backup.sh
```
Additionally, you can drop scripts into `/etc/cron.daily/`, `/etc/cron.hourly/`, etc., to have them executed automatically without writing cron syntax.

### Common Cron Pitfalls
1.  **Environment Variables:** Cron runs with a very minimal environment. It does not load your `~/.bashrc`. Always use absolute paths (e.g., `/usr/bin/python3 /opt/script.py` instead of just `python3 script.py`).
2.  **Output:** By default, cron attempts to email the output of scripts to the local user. If mail isn't configured, the output is lost. Always redirect output:
    ```text
    * * * * * /script.sh >> /var/log/cron_script.log 2>&1
    ```
