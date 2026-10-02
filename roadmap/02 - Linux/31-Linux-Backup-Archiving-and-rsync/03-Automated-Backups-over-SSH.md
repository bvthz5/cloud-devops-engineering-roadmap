# 03 — Automated Backups over SSH

Production backup automation must run non-interactively without passwords, using dedicated isolated SSH credentials.

---

## 1. Production Backup Script (`/usr/local/bin/nightly-backup.sh`)

```bash
#!/usr/bin/env bash
set -euo pipefail

BACKUP_SRC="/data/production/"
BACKUP_DEST="backupuser@10.0.1.200:/storage/backups/node01/"
SSH_KEY="/root/.ssh/id_backup_ed25519"
LOG_FILE="/var/log/nightly-backup.log"

echo "[$(date -u +'%Y-%m-%dT%H:%M:%SZ')] Starting automated backup..." >> "$LOG_FILE"

rsync -avzP \
    -e "ssh -i $SSH_KEY -o StrictHostKeyChecking=accept-new -o BatchMode=yes" \
    --delete \
    --exclude="*.log" \
    --exclude="cache/" \
    "$BACKUP_SRC" "$BACKUP_DEST" >> "$LOG_FILE" 2>&1

echo "[$(date -u +'%Y-%m-%dT%H:%M:%SZ')] Backup finished successfully!" >> "$LOG_FILE"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - rsync Synchronization](./02-rsync-Remote-Synchronization-Deep-Dive.md) | [README](./README.md) | [04 - Snapshot-Based Backups](./04-Snapshot-Based-Backups-LVM-and-Btrfs.md) |
