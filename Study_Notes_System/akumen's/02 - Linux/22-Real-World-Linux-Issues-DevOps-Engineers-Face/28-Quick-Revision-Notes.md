# 28 — Quick Revision Notes

## 1. Top 10 High-Yield Diagnostic Shortcuts

```bash
# 1. Check load average & uptime
uptime

# 2. Top memory consuming processes
ps aux --sort=-%mem | head -n 10

# 3. Top CPU consuming processes
ps aux --sort=-%cpu | head -n 10

# 4. Find open deleted files holding disk space
lsof +L1

# 5. Check inode availability
df -i

# 6. Check listening TCP/UDP sockets
ss -tulpn

# 7. Check recent kernel OOM killer events
dmesg -T | grep -i oom

# 8. Check systemd unit logs
journalctl -u service_name -n 50 --no-pager

# 9. Test remote port connectivity
nc -zv -w 3 host port

# 10. Check DNS resolution chain
dig +trace domain.com
```

## 2. Golden Troubleshooting Rules
- **Rule 1**: Never guess. Follow empirical log and metric evidence (`dmesg`, `journalctl`, `lsof`).
- **Rule 2**: `df -h` 100% full + `du -sh` small = Unlinked open file descriptor leak (`lsof +L1`).
- **Rule 3**: Exit code 137 = OOM Killed by kernel (`dmesg | grep oom`).
- **Rule 4**: `/bin/bash^M: bad interpreter` = Windows CRLF line endings (`dos2unix`).
- **Rule 5**: `Permission denied (publickey)` = Over-permissive `.ssh` permissions (`chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [27 - Incident Response Checklist](./27-Production-Incident-Response-Checklist.md) | [README](./README.md) | [README (Index)](./README.md) |
