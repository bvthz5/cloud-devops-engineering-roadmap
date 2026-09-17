# 10. Cron Security and Best Practices

## 1. Follow least privilege
Run a job as the least-privileged account that can perform the task. Do not use `root` automatically.

- **Bad design:** Everything runs in `root` cron.
- **Better:** Backup job → backup user; web maintenance → web/service account; system maintenance → root only when necessary.

## 2. Protect cron scripts
Check ownership:
```bash
ls -l /home/alice/scripts/backup.sh
```
A privileged cron job should not execute a script that an untrusted user can modify.

## 3. Protect parent directories
It is not enough to secure the script if a lower-privileged user can replace files or directories involved in the execution path.
Check:
```bash
namei -l /path/to/script.sh
```

## 4. Use absolute paths
Avoid ambiguous command resolution.
Prefer:
```cron
0 2 * * * /usr/local/bin/backup.sh
```
over:
```cron
0 2 * * * backup.sh
```

## 5. Avoid dangerous wildcard behavior
Be careful with commands such as `rm -rf`. A mistake in a path or variable can be destructive. Validate variables and use carefully controlled directories.

## 6. Quote variables in shell scripts
Prefer:
```bash
rm -- "$file"
```
over:
```bash
rm $file
```

## 7. Log important jobs
Use redirection:
```cron
>> /var/log/myjob.log 2>&1
```
with permissions appropriate to the user running the job.

## 8. Prevent overlapping runs
If a job may take longer than its interval, two instances can overlap.

Example problem:
- 02:00 → backup starts
- 02:10 → next backup starts
- 02:20 → first backup still running

For jobs where overlap is unsafe, consider a locking mechanism such as `flock`.
Example:
```cron
*/10 * * * * /usr/bin/flock -n /tmp/myjob.lock /usr/local/bin/myjob.sh
```

## 9. Make jobs idempotent where possible
An idempotent task can safely be run more than once without causing an incorrect result.

## 10. Handle failures
A script should:
- Return a meaningful exit status
- Log errors
- Avoid hiding important failures
- Clean up temporary resources where needed

## 11. Keep logs manageable
Use log rotation (`logrotate`) or retention policies for long-running scheduled jobs.

## 12. Test before scheduling production work
Run the command manually first. Then test with a harmless every-minute job.

## 13. Be careful with secrets
Avoid putting passwords directly in crontab entries.
Bad:
```cron
0 2 * * * backup --password='secret123'
```
Prefer secure credential mechanisms appropriate to the application.

## 14. Document important jobs
Add comments explaining the job's purpose:
```cron
# Nightly application backup
0 2 * * * /usr/local/bin/backup.sh
```

## 15. Consider concurrency and dependencies
If job B depends on job A finishing, two independent cron entries may not provide the required ordering. For complex dependency graphs, use a more suitable scheduler/orchestrator.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - System Wide Cron](./09-System-Wide-Cron.md) | [README](./README.md) | [11 - Cron vs Alternatives](./11-Cron-vs-Alternatives.md) |
