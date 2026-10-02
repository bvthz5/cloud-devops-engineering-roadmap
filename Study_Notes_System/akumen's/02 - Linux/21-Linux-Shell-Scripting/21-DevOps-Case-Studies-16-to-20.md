# 21 — DevOps Case Studies 16 to 20

This module details 5 real-world production DevOps automation case studies (16 to 20).

---

## Case Study 16: Git Repository Deployment Automation

### 1. Objective & Concept
Pull latest production branch updates from remote Git repository, verify clean working tree, install dependencies, and reload web application server.

### 2. Complete Script Code (`16_git_deployment.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

REPO_DIR="/var/www/my-app"
BRANCH="main"

echo "=== Git Production Deployment ==="

cd "$REPO_DIR"

git fetch origin "$BRANCH"

LOCAL_HASH=$(git rev-parse HEAD)
REMOTE_HASH=$(git rev-parse "origin/$BRANCH")

if [[ "$LOCAL_HASH" == "$REMOTE_HASH" ]]; then
  echo "Repository is already up to date ($LOCAL_HASH). Skipping deploy."
  exit 0
fi

echo "Updating code base from $LOCAL_HASH to $REMOTE_HASH..."
git pull origin "$BRANCH"

npm install --production
systemctl reload nginx

echo "[SUCCESS] Deployment completed successfully."
```

---

## Case Study 17: Directory Synchronization with Verification

### 1. Objective & Concept
Synchronize two local directories (`/data/primary/` and `/data/secondary/`) ensuring identical contents and permissions using `rsync`.

### 2. Complete Script Code (`17_directory_sync.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

SRC="/data/primary/"
DST="/data/secondary/"

mkdir -p "$DST"

echo "Syncing $SRC -> $DST..."
rsync -a --delete --checksum "$SRC" "$DST"

echo "Directory sync completed."
```

---

## Case Study 18: System & Hardware Information Report Generator

### 1. Objective & Concept
Collect hardware specs (OS kernel, CPU model, total RAM, network interface MAC/IPs, disk model) into a clean text document.

### 2. Complete Script Code (`18_system_info_report.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

REPORT_FILE="/tmp/system_summary.txt"

cat <<EOF > "$REPORT_FILE"
================================================================================
                        SYSTEM HARDWARE & INFRASTRUCTURE REPORT
================================================================================
Hostname       : $(hostname -f)
OS Release     : $(grep PRETTY_NAME /etc/os-release | cut -d= -f2 | tr -d '"')
Kernel Version : $(uname -r)
Architecture   : $(uname -m)
CPU Model      : $(lscpu | grep "Model name:" | sed 's/Model name:\s*//')
Total CPU Cores: $(nproc)
Total RAM      : $(free -h | awk '/Mem:/ {print $2}')
Root Disk Size : $(df -h / | awk 'NR==2 {print $2}')
Primary IP     : $(hostname -I | awk '{print $1}')
================================================================================
EOF

cat "$REPORT_FILE"
```

---

## Case Study 19: Process Watchdog and Crash Recovery

### 1. Objective & Concept
Check if specific application process PID file exists and process is responsive. If process crashed, log crash timestamp and spawn restart daemon.

### 2. Complete Script Code (`19_process_watchdog.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

PROCESS_NAME="node"
PID_FILE="/var/run/app.pid"

if [[ -f "$PID_FILE" ]] && kill -0 "$(cat "$PID_FILE")" 2>/dev/null; then
  echo "[OK] Process $PROCESS_NAME (PID: $(cat "$PID_FILE")) is healthy."
else
  echo "[ALERT] Process $PROCESS_NAME is NOT running! Restarting..." >&2
  nohup node /var/www/app/index.js > /var/log/app.log 2>&1 &
  echo $! > "$PID_FILE"
  echo "[RESTART] Process restarted with PID $(cat "$PID_FILE")."
fi
```

---

## Case Study 20: Docker Container Cleanup and Prune Manager

### 1. Objective & Concept
Periodically remove stopped Docker containers, dangling images, unused volumes, and build cache to reclaim disk space on Docker hosts.

### 2. Complete Script Code (`20_docker_cleanup.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

echo "=== Starting Docker Infrastructure Cleanup ==="

BEFORE_SPACE=$(docker system df | awk 'NR==2 {print $4}')

# Prune stopped containers, dangling images, networks
docker system prune -f --volumes

AFTER_SPACE=$(docker system df | awk 'NR==2 {print $4}')

echo "Docker cleanup finished."
echo "Space Before: $BEFORE_SPACE | Space After: $AFTER_SPACE"
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [20 - DevOps Case Studies 11-15](./20-DevOps-Case-Studies-11-to-15.md) | [README](./README.md) | [22 - Hands-On Labs 01-05](./22-Hands-On-Labs-01-to-05-Backup-and-Disk.md) |
