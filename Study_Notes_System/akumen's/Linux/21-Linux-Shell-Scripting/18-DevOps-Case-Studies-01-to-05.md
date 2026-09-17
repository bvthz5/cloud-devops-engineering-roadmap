# 18 — DevOps Case Studies 01 to 05

This module details 5 real-world production DevOps automation case studies with full concept explanations, step-by-step commands, script code, and sample outputs.

---

## Case Study 01: Automated Remote Server Backup using `rsync`

### 1. Objective & Concept
Automatically mirror critical application directories (`/var/www/app`) from a local web server to a remote backup storage server via SSH using `rsync`. The script must support delta synchronization, bandwidth limiting, timestamped log generation, and non-interactive SSH authentication.

### 2. Complete Script Code (`01_rsync_backup.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

# Configuration
SOURCE_DIR="/var/www/app/"
REMOTE_USER="backupuser"
REMOTE_HOST="192.168.1.150"
REMOTE_DIR="/mnt/backups/web_app/"
SSH_KEY="/home/devops/.ssh/id_ed25519"
LOG_FILE="/var/log/rsync_backup.log"

log() { echo "[$(date -u +'%Y-%m-%dT%H:%M:%SZ')] $*" | tee -a "$LOG_FILE" ; }

log "Starting rsync automated backup..."

if [[ ! -d "$SOURCE_DIR" ]]; then
  log "[ERROR] Source directory $SOURCE_DIR does not exist!" >&2
  exit 1
fi

rsync -avz --delete \
  -e "ssh -i $SSH_KEY -o StrictHostKeyChecking=no" \
  --bwlimit=5000 \
  "$SOURCE_DIR" \
  "${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}" >> "$LOG_FILE" 2>&1

log "Rsync backup completed successfully."
```

### 3. Step-by-Step Execution Commands
```bash
# 1. Make script executable
chmod +x 01_rsync_backup.sh

# 2. Run manually to test
./01_rsync_backup.sh

# 3. Schedule via Crontab (daily at 02:00 AM)
crontab -e
# Add: 0 2 * * * /opt/scripts/01_rsync_backup.sh
```

### 4. Sample Output Log
```text
[2026-09-17T22:00:01Z] Starting rsync automated backup...
sending incremental file list
./
index.html
assets/css/style.css
assets/js/app.js

sent 14,250 bytes  received 92 bytes  9,561.33 bytes/sec
total size is 452,100  speedup is 31.52
[2026-09-17T22:00:03Z] Rsync backup completed successfully.
```

---

## Case Study 02: Automated Server Maintenance & Package Updates

### 1. Objective & Concept
Automate routine Ubuntu/Debian server maintenance including updating APT repository indexes, upgrading installed packages, removing orphaned dependencies, cleaning APT cache, and checking if a system reboot is required due to kernel updates.

### 2. Complete Script Code (`02_server_maintenance.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

export DEBIAN_FRONTEND=noninteractive
LOG_FILE="/var/log/server_maintenance.log"

log() { echo "[$(date -u +'%Y-%m-%dT%H:%M:%SZ')] $*" | tee -a "$LOG_FILE" ; }

log "=== Starting System Maintenance Routine ==="

log "1. Updating APT Package Lists..."
apt-get update -y >> "$LOG_FILE" 2>&1

log "2. Upgrading Installed Packages..."
apt-get upgrade -y >> "$LOG_FILE" 2>&1

log "3. Removing Unused Dependencies (autoremove)..."
apt-get autoremove -y >> "$LOG_FILE" 2>&1

log "4. Cleaning Package Archive Cache..."
apt-get autoclean -y >> "$LOG_FILE" 2>&1

if [[ -f /var/run/reboot-required ]]; then
  log "WARNING: System reboot required due to kernel updates!"
else
  log "System update complete. No reboot required."
fi
```

---

## Case Study 03: Disk Space Monitoring & Webhook Alerter

### 1. Objective & Concept
Monitor mounted filesystems. If any partition exceeds 85% capacity, trigger an alert notification containing detailed usage breakdown to standard error and log stream.

### 2. Complete Script Code (`03_disk_monitor.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

THRESHOLD=85
ALERT_TRIGGERED=0

echo "=== Disk Space Monitoring Report ==="

while read -r LINE; do
  FILESYSTEM=$(echo "$LINE" | awk '{print $1}')
  USAGE=$(echo "$LINE" | awk '{print $5}' | tr -d '%')
  MOUNT=$(echo "$LINE" | awk '{print $6}')

  if [[ "$USAGE" -ge "$THRESHOLD" ]]; then
    echo "[ALERT] Partition '$MOUNT' ($FILESYSTEM) is at ${USAGE}% capacity (Threshold: ${THRESHOLD}%)!" >&2
    ALERT_TRIGGERED=1
  else
    echo "[OK] Partition '$MOUNT': ${USAGE}% used."
  fi
done < <(df -hP | grep -E '^/dev/')

if [[ "$ALERT_TRIGGERED" -eq 1 ]]; then
  exit 2
fi
```

---

## Case Study 04: Log Cleanup & Retention Policy Automation

### 1. Objective & Concept
Purge application log files older than 14 days in `/var/log/app/` to prevent disk space exhaustion, while maintaining an execution log of deleted file paths.

### 2. Complete Script Code (`04_log_cleanup.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

TARGET_DIR="/var/log/app"
RETENTION_DAYS=14
LOG_FILE="/var/log/log_cleanup.log"

log() { echo "[$(date -u +'%Y-%m-%dT%H:%M:%SZ')] $*" | tee -a "$LOG_FILE" ; }

log "Starting log cleanup in $TARGET_DIR (Retention: $RETENTION_DAYS days)..."

if [[ ! -d "$TARGET_DIR" ]]; then
  log "[ERROR] Target directory $TARGET_DIR does not exist." >&2
  exit 1
fi

DELETED_COUNT=0
while IFS= read -r FILE_PATH; do
  [[ -z "$FILE_PATH" ]] && continue
  log "Deleting expired log: $FILE_PATH"
  rm -f "$FILE_PATH"
  (( DELETED_COUNT++ ))
done < <(find "$TARGET_DIR" -type f -name "*.log" -mtime +"$RETENTION_DAYS")

log "Cleanup finished. Total files deleted: $DELETED_COUNT."
```

---

## Case Study 05: Bulk User Provisioning from CSV File

### 1. Objective & Concept
Provision multiple Linux system user accounts in bulk from an input CSV file (`users.csv`), setting default shells, generating random temporary passwords, and forcing password reset at first login.

### 2. Complete Script Code (`05_bulk_user_creation.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

CSV_FILE="${1:-users.csv}"

if [[ ! -f "$CSV_FILE" ]]; then
  echo "Usage: $0 <users.csv>" >&2
  exit 1
fi

# CSV Format: username,group,shell
while IFS=',' read -r USERNAME GROUP USER_SHELL || [[ -n "$USERNAME" ]]; do
  # Skip header or empty lines
  [[ "$USERNAME" == "username" ]] && continue
  [[ -z "$USERNAME" ]] && continue

  if id "$USERNAME" &>/dev/null; then
    echo "[SKIP] User '$USERNAME' already exists."
    continue
  fi

  # Create group if not exists
  getent group "$GROUP" &>/dev/null || groupadd "$GROUP"

  # Create user with primary group and shell
  useradd -m -g "$GROUP" -s "${USER_SHELL:-/bin/bash}" "$USERNAME"

  # Set random temporary password
  TEMP_PASS="$(tr -dc 'A-Za-z0-9' < /dev/urandom | head -c 12)"
  echo "${USERNAME}:${TEMP_PASS}" | chpasswd
  passwd -e "$USERNAME" &>/dev/null  # Force change password on first login

  echo "[CREATED] User '$USERNAME' added to group '$GROUP'. Temp Pass: $TEMP_PASS"
done < "$CSV_FILE"
```
