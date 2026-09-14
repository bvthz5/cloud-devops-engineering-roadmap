# 17. Cron Commands Cheat Sheet

## User Crontab Management
```bash
crontab -e                # Edit current user's crontab
crontab -l                # List current user's cron jobs
crontab -r                # Remove all cron jobs for current user
sudo crontab -u alice -e  # Edit crontab for user 'alice'
```

## Service Management
```bash
# Debian / Ubuntu
systemctl status cron
sudo systemctl start cron
sudo systemctl stop cron
sudo systemctl restart cron
sudo systemctl enable cron

# RHEL / CentOS / Fedora
systemctl status crond
sudo systemctl start crond
sudo systemctl restart crond
sudo systemctl enable crond
```

## Log Inspection
```bash
grep CRON /var/log/syslog       # Debian / Ubuntu log search
sudo tail -f /var/log/cron      # RHEL / CentOS log stream
journalctl -u cron              # Systemd journal log
```

## Permissions & Debugging
```bash
ls -l /path/to/script.sh        # Check file permissions
chmod +x /path/to/script.sh     # Make script executable
namei -l /path/to/script.sh     # Check directory tree permissions
command -v python3              # Find absolute path of binary
command -v flock                # Find flock utility path
```

## Redirection Syntax
- `> file` : Replace stdout
- `>> file` : Append stdout
- `2> file` : Replace stderr
- `2>> file` : Append stderr
- `>> file 2>&1` : Append stdout and stderr together
- `> /dev/null 2>&1` : Discard all output

## Production Templates
```cron
# Every minute
* * * * * /path/to/script.sh

# Every 5 minutes
*/5 * * * * /path/to/script.sh

# Every day at 2 AM
0 2 * * * /path/to/script.sh

# Weekdays at 9 AM
0 9 * * 1-5 /path/to/script.sh

# Sunday at midnight
0 0 * * 0 /path/to/script.sh

# Log stdout and stderr
0 2 * * * /path/to/script.sh >> /var/log/job.log 2>&1

# Prevent job overlap
*/5 * * * * /usr/bin/flock -n /tmp/job.lock /path/to/script.sh
```
