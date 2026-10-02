# 07 — Real-World Logging Production Scenarios

---

## Scenario 1: Disk Space Emergency Triggered by an Unrotated 60GB Log

### Incident Summary
A custom Python microservice writes verbose unformatted JSON logs to `/var/log/myapp.log`. The disk hits 100% full.
The engineer adds a logrotate config, but running logrotate fails because gzip runs out of disk space while trying to compress the 60GB file!

### Production Solution
1. Truncate the file safely to release disk space without restarting the service:
   ```bash
   sudo truncate -s 50M /var/log/myapp.log
   ```
2. Configure `copytruncate` with a size threshold to prevent files from ever growing beyond 200MB:
   ```text
   /var/log/myapp.log {
       size 200M
       rotate 5
       copytruncate
       compress
       missingok
   }
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Auditing Linux with auditd](./06-Auditing-Linux-with-auditd.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
