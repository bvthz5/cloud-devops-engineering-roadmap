# 08 - Automation Templates: Troubleshooting Guide

## 1. Cron Execution Fails While Terminal Manual Run Succeeds

### The Common Bug
Cron executes scripts in a minimal, stripped environment with `PATH=/usr/bin:/bin`. Utilities installed in `/usr/local/bin` (like `kubectl`, `aws`, or `jq`) fail with `command not found`.

### The Fix
Declare the explicit PATH inside the script or crontab:
```bash
PATH=/usr/local/bin:/usr/bin:/bin
```

---

## 2. Preventing Concurrent Script Execution with `flock`
```bash
# In crontab: Prevent second run if previous backup is still running:
*/10 * * * * /usr/bin/flock -n /var/run/backup.lock /usr/local/bin/backup.sh
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
