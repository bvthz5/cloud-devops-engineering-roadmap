# Module 31: Linux Backup, Archiving, and rsync

Data protection, incremental disaster recovery synchronization, and file archiving form the safety net of production operations. DevOps and SRE professionals must master high-speed file transfer (`rsync`), modern compression (`zstd`), snapshot-based zero-downtime backups, and automated replication strategies to guarantee business continuity.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Archive and compress files with `tar`, `gzip`, and modern multi-threaded `zstd`.
- Master **`rsync`** options (`-avzP`, `--delete`, `--exclude`, checksums).
- Understand the critical **rsync trailing slash nuance** (`/src` vs `/src/`).
- Automate secure incremental remote file replication over SSH.
- Execute zero-downtime database backups using **LVM snapshots**.
- Design robust Disaster Recovery architectures meeting strict **RPO** and **RTO** targets.
- Implement modern encrypted, deduplicated backups using **Restic** and **Borg**.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Archiving & Compression (tar, gzip, zstd)](./01-Archiving-and-Compression-tar-gzip-bzip2-zstd.md) | `tar` flags, preserving permissions, parallel compression with `zstd` | ✅ Complete |
| 02 | [rsync Remote Synchronization Deep Dive](./02-rsync-Remote-Synchronization-Deep-Dive.md) | Delta-transfer algorithm, bandwidth throttling, and trailing slashes | ✅ Complete |
| 03 | [Automated Backups over SSH](./03-Automated-Backups-over-SSH.md) | Non-interactive replication, SSH key isolation, and retention pruning | ✅ Complete |
| 04 | [Snapshot-Based Backups (LVM & Cloud)](./04-Snapshot-Based-Backups-LVM-and-Btrfs.md) | Atomic point-in-time snapshots for live databases without downtime | ✅ Complete |
| 05 | [Disaster Recovery: RPO & RTO](./05-Disaster-Recovery-Strategies-RPO-and-RTO.md) | 3-2-1 backup strategy, Recovery Point vs Recovery Time, immutable vaults | ✅ Complete |
| 06 | [Deduplicated Backups: Restic & Borg](./06-Deduplicated-and-Encrypted-Backups-Borg-Restic.md) | Content-addressable deduplication and client-side encryption | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | The catastrophic rsync `--delete` incident and 5TB zero-downtime migration | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Exit code analysis, partial transfers, permission errors, and dry-runs | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical backup interview questions for DevOps, SRE, and Cloud | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Fast zstd backups, incremental rsync mirror, and snapshot restoration | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Complete command cheat sheet for rsync, tar, and snapshot recovery | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 30: Log Management](../30-Linux-Log-Management-Logrotate-and-Rsyslog/README.md) | [Linux Roadmap Index](../README.md) | [01 - Archiving & Compression](./01-Archiving-and-Compression-tar-gzip-bzip2-zstd.md) |
