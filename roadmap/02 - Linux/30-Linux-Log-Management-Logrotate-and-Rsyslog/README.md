# Module 30: Linux Log Management (Logrotate, rsyslog, auditd)

Logs are the ultimate source of truth for debugging outages, tracing security breaches, and auditing compliance. Without automated log rotation, disks will quickly fill up to 100%, causing production services to crash. This module covers log management architecture, rotation policies, remote forwarding, and security auditing.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Understand the Linux logging architecture, syslog facilities, and severity levels.
- Configure **`rsyslog`** to filter, format, and securely forward logs over TCP/TLS to central collectors.
- Master **`logrotate`** syntax, retention schedules, compression, and postrotate signals.
- Choose correctly between **`create`** and **`copytruncate`** strategies.
- Ship Linux host logs to modern observability platforms (Elasticsearch, Loki, Datadog).
- Audit privileged user actions and security events using the Linux **`auditd`** subsystem.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Linux Logging Architecture & /var/log](./01-Linux-Logging-Architecture-and-var-log.md) | Facilities, severities, `/var/log` structure, and syslog RFC 5424 | ✅ Complete |
| 02 | [rsyslog Configuration & Forwarding](./02-rsyslog-Configuration-and-Remote-Forwarding.md) | Filtering rules, structured templates, and shipping logs via TCP/TLS | ✅ Complete |
| 03 | [Logrotate Deep Dive & Retention Policies](./03-Logrotate-Configuration-and-Retention-Policies.md) | Rotation schedules, size triggers, compression, and `/etc/logrotate.d/` | ✅ Complete |
| 04 | [copytruncate vs create & Signal Delivery](./04-copytruncate-vs-create-Signals.md) | File descriptor mechanics, `SIGHUP` reload, and zero-loss log rotation | ✅ Complete |
| 05 | [Centralized Log Aggregation Shippers](./05-Centralized-Log-Aggregation-Shippers.md) | Fluent Bit, Vector, and Promtail integration with Linux host logs | ✅ Complete |
| 06 | [Linux Security Auditing with auditd](./06-Auditing-Linux-with-auditd.md) | Tracking root commands, sensitive file access, and compliance auditing | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Disk fill-up triage, unrotated 50GB logs, and PCI-DSS compliance audits | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Debugging with `logrotate -d`, forced execution (`-f`), and permissions | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical log management interview questions for DevOps/SRE roles | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Build custom rotation policies, forward logs, and configure audit rules | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Directives reference for logrotate, rsyslog filters, and auditd commands | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 29: Performance Tuning](../29-Linux-Performance-Tuning-and-Observability/README.md) | [Linux Roadmap Index](../README.md) | [01 - Linux Logging Architecture](./01-Linux-Logging-Architecture-and-var-log.md) |
