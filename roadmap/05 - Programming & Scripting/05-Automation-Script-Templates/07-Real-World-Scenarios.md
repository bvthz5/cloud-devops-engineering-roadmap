# 07 - Automation Templates: Real-World Production Scenarios

## Scenario 1: The Automated Cleanup Script That Deleted Active Databases

### Incident Summary
A Junior DevOps engineer deployed a disk space cleanup template configured with `find /data/ -name "*.db" -mtime +30 -delete`. However, the production SQLite and database write-ahead log files were last modified during system creation, though actively held open in memory by the server! The script deleted active production databases during business hours.

### SRE Lessons Learned
1. Cleanup scripts must **never** delete by broad wildcard patterns in application data directories.
2. Always inspect file open handles with `lsof` or restrict cleanups strictly to `/tmp` and dedicated rotated log paths (`/var/log/`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Database Latency and Connection Prober Template](./06-Database-Latency-and-Connection-Prober-Template.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
