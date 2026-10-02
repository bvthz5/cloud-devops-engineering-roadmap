# 08 - Real-World Production Scenarios

In production DevOps environments, incorrect path references cause automated deployment failures, broken Docker containers, broken cron jobs, and broken symbolic links.

---

## 🚨 1. Scenario: Cron Job Fails Silently Due to Relative Paths

### Issue Description:
A DevOps engineer schedules a backup script in `/etc/crontab`:
`0 2 * * * root python3 backup.py > backup.log`

The script fails every night with `python3: can't open file 'backup.py': No such file or directory`.

### Root Cause:
Cron daemons execute jobs with the default working directory set to `/` or the user's root home folder (`/root`). Relative paths (`backup.py`) fail because `/root/backup.py` does not exist.

### Production Solution:
Always use absolute paths for both the interpreter and the script operands in cron entries:
`0 2 * * * root /usr/bin/python3 /opt/scripts/backup.py > /var/log/backup.log 2>&1`

---

## 🚨 2. Scenario: Broken Symbolic Links Created with Relative Paths

### Issue Description:
An administrator creates a symbolic link while inside `/tmp`:
`ln -s logfile.txt /var/log/app_log.txt`

When trying to read `/var/log/app_log.txt`, the OS throws `No such file or directory`.

### Root Cause:
The relative target `logfile.txt` is stored verbatim inside the symlink. When accessing `/var/log/app_log.txt`, Linux looks for `/var/log/logfile.txt` instead of `/tmp/logfile.txt`.

### Production Solution:
Always create symbolic links using **absolute paths**:
`ln -s /tmp/logfile.txt /var/log/app_log.txt`

---

## 🚨 3. Scenario: Docker Bind Mount Paths

Docker volume bind mounts (`-v` or `--mount`) **REQUIRE absolute host paths**:
```bash
# INVALID in Docker CLI:
docker run -v ./data:/app/data nginx

# VALID in Docker CLI (using absolute path or shell expansion):
docker run -v /home/ubuntu/data:/app/data nginx
docker run -v $(pwd)/data:/app/data nginx
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Practical Command Examples](./07-Practical-Command-Examples.md) | [README](./README.md) | [09 - Troubleshooting](./09-Troubleshooting.md) |
