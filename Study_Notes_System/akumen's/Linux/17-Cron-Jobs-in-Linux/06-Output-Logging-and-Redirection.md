# 6. Output, Errors, and Logging

## Why logging matters

A cron job may execute when nobody is watching.

If it fails silently, you may not notice.

A useful cron job should produce enough logging to answer:
- Did the job start?
- Did it finish?
- Did it fail?
- What error occurred?
- When did it run?

## Standard output and standard error

Linux commonly uses file descriptors:
- `0` = Standard input (`stdin`)
- `1` = Standard output (`stdout`)
- `2` = Standard error (`stderr`)

## Append normal output to a log
```cron
0 2 * * * /home/alice/scripts/backup.sh >> /home/alice/logs/backup.log
```
`>>` appends instead of replacing the file.

## Capture output and errors
```cron
0 2 * * * /home/alice/scripts/backup.sh >> /home/alice/logs/backup.log 2>&1
```

Meaning:
```text
command
  |
  +--> stdout ----+
  |               |
  +--> stderr --> same log
```
`2>&1` sends stderr to the same destination currently used by stdout.

## Discard output
```cron
0 2 * * * /home/alice/scripts/backup.sh > /dev/null 2>&1
```
Use this only when you deliberately want no output.

## Prefer meaningful logs inside scripts

For important jobs, logging inside the script can be more useful than silently discarding output.

Example script:
```bash
#!/bin/bash

LOG="/home/alice/logs/backup.log"

echo "[$(date)] Backup started" >> "$LOG"

if /home/alice/scripts/do-backup.sh >> "$LOG" 2>&1; then
    echo "[$(date)] Backup completed successfully" >> "$LOG"
else
    echo "[$(date)] Backup FAILED" >> "$LOG"
    exit 1
fi
```

## Log rotation

Logs can grow forever. For production systems, consider:
- `logrotate`
- Application-level rotation
- File size limits
- Retention policies

## Source foundation

The supplied source recommends redirecting output to a log file and explains `>`, `2>`, and `2>&1`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Special Shortcuts](./05-Special-Shortcuts.md) | [README](./README.md) | [07 - Environment and Paths](./07-Environment-and-Paths.md) |
