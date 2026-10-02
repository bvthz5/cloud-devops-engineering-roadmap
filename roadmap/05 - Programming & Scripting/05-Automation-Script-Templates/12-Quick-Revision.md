# 12 - Automation Templates: Quick Revision Cheat Sheet

## Essential Automation Snippets

### File Locking Cron
```bash
* * * * * /usr/bin/flock -n /var/run/myjob.lock /usr/local/bin/myjob.sh
```

### Clean Old Files (>30 days)
```bash
find /var/log/apps -type f -name "*.log" -mtime +30 -delete
```

### SSL Days Left Formula (Bash/OpenSSL)
```bash
EXPIRY=$(openssl s_client -connect google.com:443 -servername google.com </dev/null 2>/dev/null | openssl x509 -noout -enddate | cut -d= -f2)
echo "Expiry date: $EXPIRY"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [06 - PowerShell Core](../06-PowerShell-Core-for-Cloud-and-DevOps/README.md) |
