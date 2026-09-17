# 8. Testing and Troubleshooting Cron

## Troubleshooting flow
```text
Job did not run or failed
      |
      v
Is cron service running?
      |
      +-- No --> Start/fix service (systemctl start cron/crond)
      |
      v
Is job actually in the crontab?
      |
      +-- No --> Fix crontab (crontab -e)
      |
      v
Is schedule correct?
      |
      v
Is command/script executable? (chmod +x)
      |
      v
Are all paths absolute?
      |
      v
Are interpreter paths correct? (/usr/bin/python3)
      |
      v
Are permissions correct?
      |
      v
Check logs/output redirection
      |
      v
Run command manually
```

## Check the cron service

Debian/Ubuntu commonly:
```bash
systemctl status cron
```

RHEL/CentOS/Fedora commonly:
```bash
systemctl status crond
```

### Start the service if needed
On systems using systemd:
```bash
sudo systemctl start cron
```
or:
```bash
sudo systemctl start crond
```

### Enable at boot
```bash
sudo systemctl enable cron
```
or:
```bash
sudo systemctl enable crond
```

## Check your crontab
```bash
crontab -l
```

## Check system logs

Debian/Ubuntu commonly:
```bash
grep CRON /var/log/syslog
```

RHEL/CentOS commonly:
```bash
sudo tail -f /var/log/cron
```

Systemd journal:
```bash
journalctl | grep CRON
```

You can also narrow it when appropriate:
```bash
journalctl -u cron
```
or:
```bash
journalctl -u crond
```

## Test the command manually

If cron runs:
```cron
0 2 * * * /home/alice/scripts/backup.sh
```
first run manually:
```bash
/home/alice/scripts/backup.sh
```
If it fails manually, fix the script before debugging cron!

## Test every minute

For a temporary test:
```cron
* * * * * /home/alice/scripts/test.sh >> /tmp/test-cron.log 2>&1
```
Wait for a minute, then:
```bash
cat /tmp/test-cron.log
```
Remove the test job afterward.

## Check permissions
```bash
ls -l /home/alice/scripts/test.sh
```
Check directory traversal too:
```bash
namei -l /home/alice/scripts/test.sh
```

## Check command location
```bash
command -v python3
command -v curl
command -v flock
```
Use those absolute paths when appropriate.

## Common failure table
| Symptom | Likely cause |
| --- | --- |
| Nothing happens | Cron service stopped |
| Job exists but never runs | Wrong schedule |
| `command not found` | Minimal PATH |
| Permission denied | File/directory permissions |
| Works manually but not cron | Environment/working directory |
| Output missing | No logging/redirection |
| Script cannot execute | Missing executable bit (`chmod +x`) or shebang |
| Wrong user permissions | Job belongs to different user |
| Relative file missing | Unexpected working directory |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Environment and Paths](./07-Environment-and-Paths.md) | [README](./README.md) | [09 - System Wide Cron](./09-System-Wide-Cron.md) |
