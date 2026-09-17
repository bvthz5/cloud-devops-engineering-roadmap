# 04 — Disk Space Exhaustion and Deleted Open Files

## 1. Scenario
An alert triggers indicating `/var` partition is **100% full**. A DevOps engineer runs `rm /var/log/nginx/access.log` to free up space. However, `df -h` still reports `/var` as 100% full, while `du -sh /var` shows only 2 GB of files!

```text
Scenario
   ↓
Symptoms: "No space left on device" errors, df -h shows 100% full, but du -sh does not explain it
   ↓
What could cause it? Deleted files still held open by active process file descriptors!
   ↓
Diagnostic commands: df -h, du -sh /var/*, lsof +L1 (or lsof | grep deleted)
   ↓
Understand output: nginx process (PID 1842) holds file descriptor 3 to /var/log/nginx/access.log (deleted)
   ↓
Find root cause: File directory link removed, but inode space cannot be reclaimed until file handle closes
   ↓
Fix / mitigate: Truncate file handle via /proc/PID/fd/FD, or reload/restart process holding file handle
   ↓
Verify: df -h shows available storage restored
   ↓
Prevent recurrence: Use logrotate with copytruncate, or restart services after log deletion
```

## 2. Diagnosing Unlinked Open Files (`lsof +L1`)

```bash
# 1. Compare filesystem reported usage vs actual directory file usage
df -h /var
du -sh /var

# 2. List all unlinked (deleted) files still held open by running processes
lsof +L1
# Or alternative syntax:
lsof | grep -i deleted
```

### Sample `lsof +L1` Output Analysis
```text
COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NLINK  NODE NAME
nginx    1842 root    3u   REG  252,1 45892104     0 12845 /var/log/nginx/access.log (deleted)
```
- **`NLINK 0`**: Directory entry deleted (`rm`), but process `nginx` (PID `1842`) holds file descriptor `3u`.
- **`SIZE/OFF`**: File occupies ~45 GB of disk blocks on the filesystem!

## 3. How to Fix Without Restarting Application
If restarting the application is not immediately possible in production:

```bash
# Option A: Truncate file content directly via /proc filesystem (frees disk blocks instantly!)
:> /proc/1842/fd/3

# Option B: Reload target service to release old file descriptors gracefully
systemctl reload nginx
```

## 4. Logrotate Best Practice (`copytruncate`)
To prevent this issue when configuring `logrotate` for custom logs, use the `copytruncate` directive:

```text
/var/log/app/*.log {
    daily
    rotate 7
    compress
    missingok
    copytruncate
}
```
> `copytruncate` copies the active log file and truncates the original in-place, keeping open file descriptors valid without process restarts!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Memory Leaks & OOM Killer](./03-Memory-Leaks-OOM-Killer-and-Swap-Exhaustion.md) | [README](./README.md) | [05 - Inode Exhaustion & Disk I/O](./05-Inode-Exhaustion-and-Disk-IO-Bottlenecks.md) |
