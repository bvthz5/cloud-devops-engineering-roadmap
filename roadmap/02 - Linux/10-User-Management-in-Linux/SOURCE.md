# SOURCE.md - Baseline Material & Educational Scope

This document preserves the topic specification, scope, and supplied notes for **Topic 10: User Management in Linux**.

---

## 📌 Topic Specification

* **Topic Name:** User Management in Linux
* **Target Path:** `Study_Notes_System/akumen's/Linux/10-User-Management-in-Linux/`
* **System Framework:** Understand ➔ See ➔ Practice ➔ Troubleshoot ➔ Interview ➔ Revise

---

## 📋 Included Core Modules & Focus Areas

* User management fundamentals & UID/GID taxonomy (Root UID 0, System UIDs 1-999, Normal UIDs 1000+)
* `/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/gshadow` field breakdowns and security permissions
* `useradd` (low-level scriptable binary) vs `adduser` (high-level interactive script)
* Password management (`passwd`) & Password aging (`chage`)
* Account locking/unlocking (`passwd -l/-u`, `usermod -L/-U`)
* User modification (`usermod`) & User deletion (`userdel`, `userdel -r`)
* Groups, primary vs supplementary groups, `groupadd`, `groupmod`, `groupdel`, `gpasswd`
* Privilege escalation (`sudo`, `su`, `su -`), `/etc/sudoers`, `visudo`, `NOPASSWD`
* Debian `sudo` group vs RHEL `wheel` group differences
* Practical CLI command reference & automation scripts
* Real-world production scenarios (Least privilege automation, PCI-DSS compliance, container security)
* Troubleshooting guide (Locked accounts, sudoers syntax errors, process termination before `userdel`)
* Junior to Senior Interview Q&A
* Step-by-step hands-on lab
* Multiple-choice practice quiz with detailed explanations
* 5-minute quick revision cheat sheet
* Ecosystem map and related Linux topics
