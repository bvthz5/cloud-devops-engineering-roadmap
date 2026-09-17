# 20 — DevOps Case Studies 11 to 15

This module details 5 real-world production DevOps automation case studies (11 to 15).

---

## Case Study 11: CPU & Memory Usage Monitor with Automated Process Inspection

### 1. Objective & Concept
Monitor CPU and RAM usage percentages. If CPU exceeds 80% or Memory exceeds 85%, print an alert and log top 3 resource-consuming processes.

### 2. Complete Script Code (`11_cpu_mem_monitor.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

CPU_LIMIT=80
MEM_LIMIT=85

CPU_IDLE=$(top -bn1 | grep "Cpu(s)" | awk '{print $8}' | cut -d. -f1)
CPU_USAGE=$(( 100 - CPU_IDLE ))

MEM_USAGE=$(free | awk '/Mem:/ {printf "%d", ($3/$2)*100}')

echo "CPU Usage: ${CPU_USAGE}% | Memory Usage: ${MEM_USAGE}%"

if [[ "$CPU_USAGE" -ge "$CPU_LIMIT" ]] || [[ "$MEM_USAGE" -ge "$MEM_LIMIT" ]]; then
  echo "[ALERT] High resource consumption detected!" >&2
  echo "--- Top 3 CPU Consuming Processes ---"
  ps aux --sort=-%cpu | head -n 4
  echo "--- Top 3 Memory Consuming Processes ---"
  ps aux --sort=-%mem | head -n 4
  exit 1
fi
```

---

## Case Study 12: Directory Archival & Compression with Verification

### 1. Objective & Concept
Compress target directory into `.tar.gz`, calculate MD5 hash before and after archival, and verify archive readability.

### 2. Complete Script Code (`12_archival_compression.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

TARGET_DIR="${1:-/var/log/old}"
ARCHIVE_DIR="/mnt/archives"
DATE_STAMP="$(date +'%Y%m%d')"
ARCHIVE_NAME="archive_$(basename "$TARGET_DIR")_${DATE_STAMP}.tar.gz"
ARCHIVE_PATH="${ARCHIVE_DIR}/${ARCHIVE_NAME}"

mkdir -p "$ARCHIVE_DIR"

echo "Creating compressed archive: $ARCHIVE_PATH"
tar -czf "$ARCHIVE_PATH" -C "$(dirname "$TARGET_DIR")" "$(basename "$TARGET_DIR")"

echo "Testing archive integrity..."
if tar -tzf "$ARCHIVE_PATH" &>/dev/null; then
  echo "[SUCCESS] Archive verified successfully."
  md5sum "$ARCHIVE_PATH" > "${ARCHIVE_PATH}.md5"
else
  echo "[ERROR] Corrupted archive created!" >&2
  rm -f "$ARCHIVE_PATH"
  exit 1
fi
```

---

## Case Study 13: Bulk File Renaming Automation

### 1. Objective & Concept
Rename all `.jpeg` or `.TXT` files in a directory to standardized lowercase `.jpg` or `.txt` format with timestamp prefixes, avoiding accidental overwrites.

### 2. Complete Script Code (`13_bulk_file_renamer.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

TARGET_DIR="${1:-.}"
PREFIX="$(date +'%Y%m%d')"

cd "$TARGET_DIR"

for FILE in *.TXT *.txt; do
  [[ -f "$FILE" ]] || continue
  NEW_NAME="${PREFIX}_${FILE,,}"  # Convert filename to lowercase
  if [[ "$FILE" != "$NEW_NAME" ]]; then
    echo "Renaming: '$FILE' -> '$NEW_NAME'"
    mv -n "$FILE" "$NEW_NAME"
  fi
done
```

---

## Case Study 14: Network Latency & Connectivity Diagnostic Checker

### 1. Objective & Concept
Ping a list of critical gateway IP addresses, calculate packet loss percentage and average RTT latency, reporting unreachable nodes.

### 2. Complete Script Code (`14_network_check.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

HOSTS=("8.8.8.8" "1.1.1.1" "192.168.1.1")

echo "=== Network Connectivity Diagnostic ==="

for HOST in "${HOSTS[@]}"; do
  if ping -c 3 -W 2 "$HOST" &>/dev/null; then
    RTT=$(ping -c 3 "$HOST" | awk -F'/' 'END {print $5}')
    echo "[ONLINE] $HOST - Avg RTT: ${RTT} ms"
  else
    echo "[OFFLINE] $HOST is unreachable!" >&2
  fi
done
```

---

## Case Study 15: Automated Systemd Service Watchdog & Self-Healer

### 1. Objective & Concept
Check if target system daemon (e.g. `nginx`, `docker`, `mysql`) is active. If inactive, attempt service restart and send critical error alert if restart fails.

### 2. Complete Script Code (`15_service_auto_restart.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

SERVICES=("nginx" "docker")

for SERVICE in "${SERVICES[@]}"; do
  if systemctl is-active --quiet "$SERVICE"; then
    echo "[OK] Service '$SERVICE' is running."
  else
    echo "[WARN] Service '$SERVICE' is down! Attempting restart..." >&2
    systemctl restart "$SERVICE" || true
    
    sleep 2
    if systemctl is-active --quiet "$SERVICE"; then
      echo "[RECOVERED] Service '$SERVICE' restarted successfully."
    else
      echo "[CRITICAL] Failed to restart service '$SERVICE'!" >&2
      exit 1
    fi
  fi
done
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [19 - DevOps Case Studies 06-10](./19-DevOps-Case-Studies-06-to-10.md) | [README](./README.md) | [21 - DevOps Case Studies 16-20](./21-DevOps-Case-Studies-16-to-20.md) |
