# 24 — Linux Package Management: APT, YUM/DNF, APK, and Source Compilation

Welcome to the definitive guide on **Linux Package Management**. In cloud infrastructure and DevOps engineering, installing, upgrading, securing, and maintaining software packages across diverse distributions (Ubuntu, Debian, RHEL, Rocky Linux, Alpine) is a fundamental daily operation.

---

## 📚 Module Overview & Syllabus

This module covers the complete software packaging ecosystem across the three major Linux families:

1. **Package Fundamentals & Architecture:** How packages are assembled, dependency graphs, metadata headers, and signature verification.
2. **Debian & Ubuntu Ecosystem:** High-level package management with `apt` / `apt-get`, low-level package operations with `dpkg`, repository mirror configurations, and Personal Package Archives (PPAs).
3. **Red Hat Enterprise Linux (RHEL) & Rocky Linux Ecosystem:** Next-generation package management with `dnf` / `yum`, binary package management with `rpm`, Extra Packages for Enterprise Linux (EPEL), and AppStream modularity.
4. **Alpine Linux & Container Optimization:** Lightweight package management with `apk`, Alpine repositories, and writing ultra-lean Docker images without package cache bloat.
5. **Compiling Software from Source:** The classic GNU Autotools pipeline (`./configure`, `make`, `make install`), shared library linking (`ldconfig`), and safe removal with `checkinstall`.
6. **Security & Automated Patching:** Automated unattended security updates (`unattended-upgrades`, `dnf-automatic`), checking for pending reboots (`needrestart`), and CVE mitigation.
7. **Real-World Scenarios:** Production outage case studies (broken dependencies, GPG key expirations, `/var` partition filling up).
8. **Troubleshooting:** Fixing locked databases (`/var/lib/dpkg/lock-frontend`), repairing corrupt RPM databases, and resolving dependency conflicts.
9. **Interview Prep, Hands-on Labs & Quick Revision:** Junior to Staff SRE interview questions, command-by-command terminal exercises, and a side-by-side command translation cheat sheet.

---

## 📂 Module Files Directory

| File | Title | Description |
|---|---|---|
| [`01-Package-Management-Fundamentals-and-Package-Types.md`](./01-Package-Management-Fundamentals-and-Package-Types.md) | Package Fundamentals | Binary packages (.deb, .rpm, .apk) vs source code, dependencies, and checksums. |
| [`02-APT-and-DPKG-Deep-Dive-Debian-Ubuntu.md`](./02-APT-and-DPKG-Deep-Dive-Debian-Ubuntu.md) | APT & DPKG (Debian/Ubuntu) | Sources list, PPAs, GPG keyrings, pinning, and package lifecycle. |
| [`03-YUM-DNF-and-RPM-Deep-Dive-RHEL-CentOS-Rocky.md`](./03-YUM-DNF-and-RPM-Deep-Dive-RHEL-CentOS-Rocky.md) | YUM, DNF & RPM (RHEL/Rocky) | DNF architecture, EPEL repositories, module streams, and RPM inspection. |
| [`04-APK-Package-Manager-Alpine-Linux-and-Containers.md`](./04-APK-Package-Manager-Alpine-Linux-and-Containers.md) | APK & Alpine Containers | Lightweight package management for Docker, `--no-cache`, and musl vs glibc. |
| [`05-Compiling-and-Installing-Software-from-Source.md`](./05-Compiling-and-Installing-Software-from-Source.md) | Source Compilation | C/C++ toolchains, configure scripts, makefile targets, and checkinstall. |
| [`06-Automated-Security-Updates-and-Patching.md`](./06-Automated-Security-Updates-and-Patching.md) | Security & Patching | Unattended upgrades, dnf-automatic, needrestart, and kernel hotpatching. |
| [`07-Real-World-Scenarios.md`](./07-Real-World-Scenarios.md) | Real-World Scenarios | Production outages: broken dependencies, GPG key rotation failures, disk exhaustion. |
| [`08-Troubleshooting.md`](./08-Troubleshooting.md) | Troubleshooting Guide | Resolving dpkg lock errors, broken dependencies, and RPM database corruption. |
| [`09-Interview-QA.md`](./09-Interview-QA.md) | Interview Questions & Answers | 10 Senior/Staff SRE interview questions on package management. |
| [`10-Hands-On-Practice.md`](./10-Hands-On-Practice.md) | Hands-On Practice Labs | 6 Step-by-step terminal labs on custom repos, source builds, and unattended updates. |
| [`11-MCQ.md`](./11-MCQ.md) | Multiple Choice Questions | 12 Self-assessment questions with expandable technical rationales. |
| [`12-Quick-Revision.md`](./12-Quick-Revision.md) | Quick Revision Cheat Sheet | Cross-distribution command translation table (APT vs DNF vs APK). |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| - | You are here | [01 - Package Management Fundamentals and Package Types](./01-Package-Management-Fundamentals-and-Package-Types.md) |
