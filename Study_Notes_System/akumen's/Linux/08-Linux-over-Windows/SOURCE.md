# SOURCE: Linux over Windows

This document outlines the origin, foundational inputs, and structural expansions applied to create the **08-Linux-over-Windows** topic within the **Akumen Study System**.

---

## 📌 Primary Inputs & Scope

The foundation of this topic is built upon architectural and strategic comparison between Linux and Windows Server platforms:

1. **Strategic & Financial Factors:**
   - Open-source GPL licensing vs Proprietary per-core licensing & CALs.
   - Cloud compute TCO reduction (eliminating Windows instance surcharges).
2. **Performance & Resource Efficiency:**
   - Headless server execution without GUI memory overhead.
   - Lightweight RAM footprint (< 500 MB idle vs 2+ GB for Windows Server).
   - Unified task model (`clone()`) vs heavy process creation overhead.
3. **Security & Reliability:**
   - POSIX permission model (`rwx`, no automatic execution of downloaded files).
   - Isolated plain-text configuration files (`/etc`) vs monolithic Registry.
   - Uptime stability and live kernel patching (no mandatory update reboots).
4. **DevOps & Containerization:**
   - Native Linux kernel container primitives (`cgroups` & `namespaces`).
   - CLI-first text streaming philosophy, SSH remote management, Ansible, Docker, Kubernetes.

---

## 🛠️ Educational Expansions & DevOps Extensions

To transform general platform comparisons into a production-grade DevOps study guide, the original prompt material was systematically expanded to include:

- **Corrected Folder Numbering:** Verified topic sequence (`08-Linux-over-Windows`).
- **Cross-Platform Failure Modes:** Troubleshooting CRLF vs LF line endings (`dos2unix`, `sed`), case sensitivity in Git and imports, and path separator syntax.
- **Equivalent Command Reference:** Direct mapping of Linux Bash commands to Windows PowerShell equivalents (`ls` vs `Get-ChildItem`, `ps` vs `Get-Process`, `ss` vs `Get-NetTCPConnection`).
- **Real-World Case Studies:** Production migration of .NET workloads to Linux containers on AWS EKS and serverless cold-start benchmarks.
- **Assessment Suite:** Technical interview questions, hands-on terminal labs, multiple-choice questions, and quick revision cheat sheets.

---

## 📂 Topic File Index

- [`README.md`](./README.md)
- [`01-Why-Linux-Is-Preferred.md`](./01-Why-Linux-Is-Preferred.md)
- [`02-Cost-Effectiveness-and-Licensing.md`](./02-Cost-Effectiveness-and-Licensing.md)
- [`03-Performance-Efficiency-and-Resource-Usage.md`](./03-Performance-Efficiency-and-Resource-Usage.md)
- [`04-Security-Permissions-and-Reliability.md`](./04-Security-Permissions-and-Reliability.md)
- [`05-Linux-vs-Windows-Comparison-Matrix.md`](./05-Linux-vs-Windows-Comparison-Matrix.md)
- [`06-Linux-in-DevOps-Cloud-and-Containers.md`](./06-Linux-in-DevOps-Cloud-and-Containers.md)
- [`07-CLI-vs-GUI-Philosophies.md`](./07-CLI-vs-GUI-Philosophies.md)
- [`08-Practical-Commands-and-Tools.md`](./08-Practical-Commands-and-Tools.md)
- [`09-Real-World-Production-Scenarios.md`](./09-Real-World-Production-Scenarios.md)
- [`10-Troubleshooting.md`](./10-Troubleshooting.md)
- [`11-Interview-QA.md`](./11-Interview-QA.md)
- [`12-Hands-On-Practice.md`](./12-Hands-On-Practice.md)
- [`13-MCQ.md`](./13-MCQ.md)
- [`14-Quick-Revision.md`](./14-Quick-Revision.md)
- [`15-Related-Topics.md`](./15-Related-Topics.md)
---

| Back to Index |
| :---: |
| [README](./README.md) |
