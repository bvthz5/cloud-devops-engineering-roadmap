# Module 27: systemd Service Management and Journald

`systemd` is the standard init system, service supervisor, and service manager for virtually all modern Linux distributions (Debian, Ubuntu, Red Hat, CentOS, Rocky Linux, SUSE, Arch). It runs as **PID 1**, boots the operating system in parallel, manages daemon lifecycles, enforces cgroups v2 resource limits, provides modern timer automation, and aggregates structured system logs via `systemd-journald`.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Understand `systemd` architecture, PID 1 execution, and unit types (`.service`, `.timer`, `.mount`, `.target`).
- Author production-ready, fault-tolerant custom `.service` units for microservices and background daemons.
- Sandbox and harden services using security directives (`ProtectSystem`, `NoNewPrivileges`, `PrivateTmp`).
- Replace legacy `crontab` jobs with modern, observable, and monotonic `systemd` timers.
- Master structured logging and log querying with `journalctl`.
- Enforce CPU and memory resource limits on services using Linux cgroups v2.
- Troubleshoot service crashes, boot bottlenecks (`systemd-analyze blame`), and drop-in overrides.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [systemd Architecture & PID 1](./01-systemd-Architecture-and-PID-1.md) | Init lifecycle, target dependency graph, parallel boot, and unit types | ✅ Complete |
| 02 | [Writing Custom systemd Service Units](./02-Writing-Custom-systemd-Service-Units.md) | `[Unit]`, `[Service]`, `[Install]`, service types, and lifecycle control | ✅ Complete |
| 03 | [Hardening & Sandboxing Services](./03-Hardening-and-Sandboxing-Services.md) | `systemd-analyze security`, filesystem isolation, and capability dropping | ✅ Complete |
| 04 | [systemd Timers: Modern Cron Replacement](./04-systemd-Timers-The-Modern-Cron-Replacement.md) | Real-time & monotonic timers, `OnCalendar`, jitter, and failure handling | ✅ Complete |
| 05 | [journald Deep Dive & Structured Logging](./05-journald-Deep-Dive-and-Structured-Logging.md) | Binary logging, filtering with `journalctl`, retention, and log forwarding | ✅ Complete |
| 06 | [cgroups v2 Resource Limits in systemd](./06-cgroups-v2-Resource-Limits-in-systemd.md) | `CPUQuota`, `MemoryMax`, `TasksMax`, slices, and `systemd-cgtop` | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Crash recovery loops, memory leak OOM mitigation, and zero-downtime reloads | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Exit code analysis, boot bottlenecks, zombie reaping, and drop-in overrides | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical interview questions for DevOps, SRE, and Systems Engineers | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Build hardened services, schedule calendar timers, and test cgroups OOM | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Complete CLI matrix for `systemctl`, `journalctl`, and unit directives | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 26: Linux Firewalls](../26-Linux-Firewalls-iptables-nftables-and-UFW/README.md) | [Linux Roadmap Index](../README.md) | [01 - systemd Architecture](./01-systemd-Architecture-and-PID-1.md) |
