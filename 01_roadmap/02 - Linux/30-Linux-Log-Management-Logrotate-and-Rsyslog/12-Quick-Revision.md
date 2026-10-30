# 12 — Quick Revision Cheat Sheet: Log Management

---

## 1. Commands & Reference

```bash
# Logrotate Debugging
sudo logrotate -d /etc/logrotate.conf   # Dry run test
sudo logrotate -f /etc/logrotate.conf   # Force rotation now
cat /var/lib/logrotate/status          # Rotation timestamp state

# Audit Log Search
sudo ausearch -k my_key                # Search by audit key
sudo aureport -au                      # Auth report
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (31-Linux-Backup-Archiving-and-rsync) →](../31-Linux-Backup-Archiving-and-rsync/01-Archiving-and-Compression-tar-gzip-bzip2-zstd.md) |
