# 01 - Disk Space Watchdog and Log Cleaner Template

```bash
#!/usr/bin/env bash
# /usr/local/bin/disk_watchdog.sh
set -euo pipefail

THRESHOLD_PERCENT=85
TARGET_MOUNT="/"
LOG_DIR="/var/log/apps"
RETENTION_DAYS=7

CURRENT_USAGE=$(df -h "${TARGET_MOUNT}" | awk 'NR==2 {print $5}' | tr -d '%')

if [ "${CURRENT_USAGE}" -ge "${THRESHOLD_PERCENT}" ]; then
    echo "[ALERT] Disk usage on ${TARGET_MOUNT} is at ${CURRENT_USAGE}% (Threshold: ${THRESHOLD_PERCENT}%)"
    echo "Purging logs older than ${RETENTION_DAYS} days in ${LOG_DIR}..."
    
    if [ -d "${LOG_DIR}" ]; then
        find "${LOG_DIR}" -type f -name "*.log" -mtime +"${RETENTION_DAYS}" -exec rm -f {} +
    fi

    NEW_USAGE=$(df -h "${TARGET_MOUNT}" | awk 'NR==2 {print $5}' | tr -d '%')
    echo "Cleanup complete. New disk usage: ${NEW_USAGE}%"
else
    echo "[OK] Disk usage on ${TARGET_MOUNT} is healthy (${CURRENT_USAGE}%)"
fi
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-APIs-REST-gRPC-and-Webhooks)](../04-APIs-REST-gRPC-and-Webhooks/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Cloud Snapshot and Backup Retention Template →](./02-Cloud-Snapshot-and-Backup-Retention-Template.md) |
