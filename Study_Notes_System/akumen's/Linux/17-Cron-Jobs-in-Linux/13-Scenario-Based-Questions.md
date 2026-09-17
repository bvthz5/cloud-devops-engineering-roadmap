# 13. Scenario-Based Questions

## Scenario 1 — Nightly backup
A company wants `/usr/local/bin/backup.sh` to run every day at 2:00 AM.
- **Answer:**
  ```cron
  0 2 * * * /usr/local/bin/backup.sh
  ```

## Scenario 2 — Every 15 minutes
A monitoring script must run every 15 minutes.
- **Answer:**
  ```cron
  */15 * * * * /usr/local/bin/check.sh
  ```

## Scenario 3 — Weekday report
A report must run at 9 AM Monday through Friday.
- **Answer:**
  ```cron
  0 9 * * 1-5 /usr/local/bin/report.sh
  ```

## Scenario 4 — Cron works manually but not from cron
The administrator runs `/home/alice/scripts/backup.sh` manually and it works, but cron fails.
- **Likely causes:** Relative paths, missing environment variables, missing `PATH`, wrong working directory, permissions, different user context.

## Scenario 5 — `python: command not found`
Cron entry: `*/10 * * * * python /home/alice/check.py`
- **Problem:** Cron does not have the same `PATH` as the interactive shell.
- **Fix:** Use full path: `*/10 * * * * /usr/bin/python3 /home/alice/check.py`

## Scenario 6 — Silent failure
A backup appears not to run and there is no visible error.
- **Fix:** Add redirection: `0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1` and inspect logs.

## Scenario 7 — Cron service stopped
- **Investigation:** `systemctl status cron` or `systemctl status crond`
- **Fix:** `sudo systemctl start cron` and `sudo systemctl enable cron`

## Scenario 8 — User deleted every cron job accidentally
The administrator ran `crontab -r`.
- **Lesson:** `crontab -r` removes all jobs in that user's crontab. Always use `crontab -e` to edit or comment out specific jobs.

## Scenario 9 — Jobs overlap
A job runs every 5 minutes but sometimes takes 20 minutes.
- **Solution:** Use locking with `flock`:
  ```cron
  */5 * * * * /usr/bin/flock -n /tmp/job.lock /usr/local/bin/job.sh
  ```

## Scenario 10 — Root security problem
A root cron job runs `/root/backup.sh`, but an unprivileged user can modify a file that the script executes.
- **Danger:** The unprivileged user can gain unauthorized root access. Secure script ownership, parent directories, and included files.

## Scenario 11 — Laptop was powered off
A daily cron job should have run while the laptop was off.
- **Answer:** Ordinary cron skips missed jobs. Use `anacron` or a `systemd timer` with persistent configuration.

## Scenario 12 — Complex dependency
Job B must start only after Job A succeeds.
- **Answer:** Do not use two independent cron entries. Use a wrapper script, locking/coordination, or a workflow tool.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Hands On Practice](./12-Hands-On-Practice.md) | [README](./README.md) | [14 - Interview Questions](./14-Interview-Questions.md) |
