# 7. Fail2Ban Intrusion Prevention

## What is Fail2Ban?
`Fail2Ban` scans system log files (e.g., `/var/log/auth.log`) for suspicious activity (repeated password failures) and dynamically updates firewall rules to block the offending IP address.

## Installation & Configuration

Install Fail2Ban:
```bash
sudo apt install fail2ban -y   # Debian/Ubuntu
sudo dnf install fail2ban -y   # RHEL/CentOS
```

Create custom jail configuration (`/etc/fail2ban/jail.local`):
```ini
[DEFAULT]
# Ban IP for 1 hour (3600 seconds)
bantime  = 3600

# Find failures within 10 minutes window
findtime = 600

# Max retries before ban
maxretry = 5

# Override SSH jail
[sshd]
enabled = true
port    = 2222
logpath = %(sshd_log)s
backend = %(sshd_backend)s
```

## Management Commands
```bash
# Restart Fail2Ban
sudo systemctl restart fail2ban

# Check jail status
sudo fail2ban-client status sshd

# Unban an IP address manually
sudo fail2ban-client set sshd unbanip 192.168.1.100
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Firewall Configuration UFW NFTables](./06-Firewall-Configuration-UFW-NFTables.md) | [README](./README.md) | [08 - File Permissions and Umask](./08-File-Permissions-and-Umask.md) |
