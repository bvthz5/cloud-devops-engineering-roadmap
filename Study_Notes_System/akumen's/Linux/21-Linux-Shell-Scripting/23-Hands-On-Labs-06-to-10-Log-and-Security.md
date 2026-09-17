# 23 — Hands-On Labs 06 to 10: Log Analysis and Security

## Lab 06: Generating & Publishing System Health HTML Dashboards

### Step-by-Step Instructions
1. Install Nginx web server: `sudo apt install -y nginx`.
2. Save health report script to `/usr/local/bin/gen_html_report.sh`.
3. Configure script output path to `/var/www/html/index.html`.
4. Run script and open browser to `http://localhost` to inspect dashboard UI.

---

## Lab 07: Database Automated Backups & Gzip Compression

### Step-by-Step Instructions
1. Install MySQL/MariaDB or PostgreSQL client.
2. Create dedicated backup user with read-only dump privileges.
3. Pass database password securely via environment variable `MYSQL_PWD` rather than command-line arguments.
4. Verify created archive with `gzip -t /var/backups/mysql/*.sql.gz`.

---

## Lab 08: Monitoring Multi-Endpoint HTTP Availability

### Step-by-Step Instructions
1. Create `urls.txt` containing valid and invalid web endpoints.
2. Use `curl -s -o /dev/null -w "%{http_code}"` inside a `while read` loop.
3. Test failure branch by including unreachable IP `http://10.255.255.1`.

---

## Lab 09: Checking SSL/TLS Certificate Expiry Reminders

### Step-by-Step Instructions
1. Run `openssl s_client -connect google.com:443` pipeline to extract expiration dates.
2. Calculate remaining days using Unix timestamps (`date +%s`).
3. Set test threshold to 1000 days to verify alert trigger logic.

---

## Lab 10: Detecting SSH Brute-Force Log Patterns

### Step-by-Step Instructions
1. Generate test failed login log entries using `logger -t sshd "Failed password for root from 192.168.1.50 port 54321 ssh2"`.
2. Execute SSH log parsing script.
3. Verify output extracts IP `192.168.1.50` and counts occurrences correctly.
