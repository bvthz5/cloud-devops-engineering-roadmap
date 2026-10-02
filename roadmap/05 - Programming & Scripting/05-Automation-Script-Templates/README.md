# Module 05: Production-Ready Automation Script Templates

Welcome to **Module 05: Production-Ready Automation Script Templates**. Theory is meaningless without tested, hardened implementation templates. This module provides complete, copy-pasteable, production-ready automation scripts used by senior DevOps engineers daily.

---

## 🎯 Learning Objectives

By the end of this module, you will master and possess production templates for:
1. **Automated Log Rotation & Disk Space Watchdog:** Monitoring filesystem thresholds and cleaning old logs.
2. **Cloud Volume Snapshot & Retention Orchestrator:** Automating daily backups and purging backups older than $N$ days.
3. **Kubernetes Pod Health Auto-Remediator:** Detecting CrashLoopBackOff pods and executing automated diagnostic collections.
4. **SSL/TLS Certificate Expiration Alert Bot:** Scanning domain portfolios and alerting 30 days before cert expiration.
5. **Slack / PagerDuty Incident Alert Dispatcher:** Formatting and broadcasting structured cards to incident channels.
6. **Database Connection Pool & Latency Health Prober:** Measuring database response times and connection saturation.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Disk Space Watchdog & Log Cleaner](./01-Disk-Space-Watchdog-and-Log-Cleaner-Template.md) | Automated Bash script monitoring disk utilization thresholds |
| 02 | [Cloud Snapshot & Backup Retention](./02-Cloud-Snapshot-and-Backup-Retention-Template.md) | Python Boto3 script taking daily EBS snapshots and pruning old backups |
| 03 | [Kubernetes Pod Health Auto-Remediator](./03-Kubernetes-Pod-Health-Auto-Remediator-Template.md) | Python/Bash script restarting crashloops and capturing logs to S3 |
| 04 | [SSL/TLS Certificate Expiration Scanner](./04-SSL-TLS-Certificate-Expiration-Scanner-Template.md) | Python script probing domains and alerting 30 days prior to expiry |
| 05 | [Slack & PagerDuty Alert Dispatcher](./05-Slack-and-PagerDuty-Alert-Dispatcher-Template.md) | Reusable incident alert webhook script with rich message attachments |
| 06 | [Database Latency & Connection Prober](./06-Database-Latency-and-Connection-Prober-Template.md) | Probing PostgreSQL/MySQL pool saturation and transaction latencies |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Runaway cleanup script deleting active database files, snapshot bill shock |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Debugging cron environment issues, lockfile race conditions |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps automation interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Testing the disk space watchdog and Slack alert dispatcher locally |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Template quick reference, cron syntax, lockfile snippet |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - APIs & Webhooks](../04-APIs-REST-gRPC-and-Webhooks/README.md) | [README](./README.md) | [01 - Disk Watchdog](./01-Disk-Space-Watchdog-and-Log-Cleaner-Template.md) |
