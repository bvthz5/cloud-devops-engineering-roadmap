# 19 — DevOps Case Studies 06 to 10

This module details 5 real-world production DevOps automation case studies (06 to 10).

---

## Case Study 06: System Health HTML Report Generator

### 1. Objective & Concept
Generate a formatted HTML system dashboard report containing hostname, uptime, CPU usage, memory utilization, disk space, and top 5 processes by memory, suitable for emailing or publishing to an internal web server.

### 2. Complete Script Code (`06_system_health_report.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

OUTPUT_HTML="/var/www/html/system_report.html"

HOSTNAME="$(hostname)"
UPTIME="$(uptime -p)"
CPU_LOAD="$(top -bn1 | grep 'Cpu(s)' | awk '{print $2 + $4}')%"
MEM_USED="$(free -m | awk '/Mem:/ {printf "%.1f%% (%d MB / %d MB)", ($3/$2)*100, $3, $2}')"
DISK_USAGE="$(df -h / | awk 'NR==2 {print $5 " (" $3 " / " $2 ")"}')"

cat <<EOF > "$OUTPUT_HTML"
<!DOCTYPE html>
<html>
<head>
  <title>System Health Report - $HOSTNAME</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f4f6f9; color: #333; margin: 20px; }
    h1 { color: #0056b3; }
    .card { background: white; padding: 15px; margin-bottom: 15px; border-radius: 5px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
    table { width: 100%; border-collapse: collapse; }
    th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
    th { background: #0056b3; color: white; }
  </style>
</head>
<body>
  <h1>System Health Dashboard: $HOSTNAME</h1>
  <p>Report Generated: $(date -u)</p>
  
  <div class="card">
    <h2>Metrics Overview</h2>
    <ul>
      <li><strong>Uptime:</strong> $UPTIME</li>
      <li><strong>CPU Usage:</strong> $CPU_LOAD</li>
      <li><strong>Memory Usage:</strong> $MEM_USED</li>
      <li><strong>Root Disk Usage:</strong> $DISK_USAGE</li>
    </ul>
  </div>

  <div class="card">
    <h2>Top 5 Memory-Consuming Processes</h2>
    <table>
      <tr><th>PID</th><th>User</th><th>%CPU</th><th>%MEM</th><th>Command</th></tr>
$(ps aux --sort=-%mem | head -n 6 | tail -n 5 | awk '{print "      <tr><td>"$2"</td><td>"$1"</td><td>"$3"</td><td>"$4"</td><td>"$11"</td></tr>"}')
    </table>
  </div>
</body>
</html>
EOF

echo "HTML report generated: $OUTPUT_HTML"
```

---

## Case Study 07: Automated MySQL Database Backup & Compression

### 1. Objective & Concept
Dump all PostgreSQL or MySQL databases using `mysqldump` / `pg_dump`, compress the SQL dump file with `gzip`, verify archive integrity, and save to `/var/backups/mysql/`.

### 2. Complete Script Code (`07_mysql_backup.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

DB_USER="root"
DB_PASS="${MYSQL_PWD:-secretpassword}"
BACKUP_DIR="/var/backups/mysql"
TIMESTAMP="$(date +'%Y%m%d_%H%M%S')"
BACKUP_FILE="${BACKUP_DIR}/all_databases_${TIMESTAMP}.sql.gz"

mkdir -p "$BACKUP_DIR"

echo "[$(date)] Starting MySQL backup dump..."

# Use mysqldump to dump all databases
mysqldump -u"$DB_USER" -p"$DB_PASS" --all-databases | gzip -9 > "$BACKUP_FILE"

chmod 600 "$BACKUP_FILE"
echo "[$(date)] Backup successfully saved to $BACKUP_FILE (Size: $(du -h "$BACKUP_FILE" | cut -f1))"
```

---

## Case Study 08: Website Availability & HTTP Status Monitor

### 1. Objective & Concept
Check the HTTP status code of multiple target URLs listed in a text file (`urls.txt`). If a site returns a non-200 code or times out after 5 seconds, log an alert and return exit code `1`.

### 2. Complete Script Code (`08_website_monitor.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

URLS=("https://google.com" "https://github.com" "http://localhost:8080/health")
FAILED=0

echo "=== Website Health Availability Check ==="

for URL in "${URLS[@]}"; do
  HTTP_CODE=$(curl -s -o /dev/null -w "%{http_code}" --connect-timeout 5 "$URL" || echo "000")
  
  if [[ "$HTTP_CODE" -ge 200 ]] && [[ "$HTTP_CODE" -lt 400 ]]; then
    echo "[ONLINE] $URL returned HTTP $HTTP_CODE"
  else
    echo "[OFFLINE] ALERT: $URL returned HTTP $HTTP_CODE!" >&2
    FAILED=1
  fi
done

if [[ "$FAILED" -eq 1 ]]; then
  exit 1
fi
```

---

## Case Study 09: SSL/TLS Certificate Expiry Checker

### 1. Objective & Concept
Inspect the SSL/TLS certificate of a domain name on port 443 using `openssl`, extract the expiration date, calculate remaining days, and alert if the certificate expires within 30 days.

### 2. Complete Script Code (`09_ssl_expiry_check.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

DOMAIN="${1:-example.com}"
PORT="443"
WARN_DAYS=30

EXPIRY_DATE=$(openssl s_client -servername "$DOMAIN" -connect "${DOMAIN}:${PORT}" </dev/null 2>/dev/null | openssl x509 -noout -enddate | cut -d= -f2)

if [[ -z "$EXPIRY_DATE" ]]; then
  echo "[ERROR] Failed to fetch SSL certificate for $DOMAIN" >&2
  exit 1
fi

EXPIRY_EPOCH=$(date -d "$EXPIRY_DATE" +%s)
NOW_EPOCH=$(date +%s)
DAYS_LEFT=$(( (EXPIRY_EPOCH - NOW_EPOCH) / 86400 ))

echo "Domain: $DOMAIN | Expiry Date: $EXPIRY_DATE | Days Remaining: $DAYS_LEFT"

if [[ "$DAYS_LEFT" -le "$WARN_DAYS" ]]; then
  echo "[WARNING] SSL certificate for $DOMAIN expires in $DAYS_LEFT days!" >&2
  exit 2
fi
```

---

## Case Study 10: SSH Failed Authentication Log Analyzer

### 1. Objective & Concept
Parse `/var/log/auth.log` or `journalctl -u sshd` to count failed SSH login attempts grouped by IP address, identifying potential brute-force attacks exceeding 10 failed attempts.

### 2. Complete Script Code (`10_ssh_log_analyzer.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

LOG_FILE="/var/log/auth.log"
THRESHOLD=10

echo "=== SSH Failed Login Analysis ==="

if [[ ! -f "$LOG_FILE" ]]; then
  echo "[INFO] Reading from journalctl..."
  FAILED_IPS=$(journalctl -u ssh | grep "Failed password" | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr)
else
  FAILED_IPS=$(grep "Failed password" "$LOG_FILE" | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr)
fi

echo "$FAILED_IPS" | while read -r COUNT IP; do
  [[ -z "$COUNT" ]] && continue
  if [[ "$COUNT" -ge "$THRESHOLD" ]]; then
    echo "[SECURITY ALERT] IP $IP has $COUNT failed login attempts!" >&2
  else
    echo "IP $IP: $COUNT failed attempts"
  fi
done
```
