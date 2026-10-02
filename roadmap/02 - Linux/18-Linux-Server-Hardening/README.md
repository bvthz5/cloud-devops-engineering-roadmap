# Topic 18 — Linux Server Hardening

## Objective
Master production-grade Linux server hardening techniques to minimize attack surfaces, secure user access, configure firewalls, harden the Linux kernel, and enforce audit monitoring.

## What this topic covers
- Hardening fundamentals & Defense-in-Depth
- Package & patch management
- User account locking & PAM password policies
- SSH server hardening
- Host firewall configuration (UFW / nftables)
- Fail2Ban brute-force intrusion prevention
- Secure file permissions, umask, & SUID/SGID audit
- Service minimization & port restriction
- Kernel network stack hardening via `sysctl`
- Linux Audit Framework (`auditd`) & log retention
- File Integrity Monitoring (AIDE) & rootkit scanners (`rkhunter`, `chkrootkit`)
- Automatic security patches (`unattended-upgrades` / `dnf-automatic`)
- Time synchronization (`chrony`) & Mandatory Access Control (SELinux / AppArmor)
- Hands-on hardening labs & lockout troubleshooting
- Interview Q&A, MCQs, Cheat Sheet, & Verification Checklist

## Recommended learning order
1. 01-Hardening-Fundamentals.md
2. 02-Patch-Management.md
3. 03-User-and-Account-Security.md
4. 04-Password-Policies-and-PAM.md
5. 05-SSH-Hardening.md
6. 06-Firewall-Configuration-UFW-NFTables.md
7. 07-Fail2Ban-Intrusion-Prevention.md
8. 08-File-Permissions-and-Umask.md
9. 09-SUID-SGID-and-World-Writable-Files.md
10. 10-Services-and-Port-Hardening.md
11. 11-Kernel-Hardening-with-Sysctl.md
12. 12-Auditd-and-Log-Monitoring.md
13. 13-File-Integrity-AIDE-Rootkit-Checkers.md
14. 14-Automatic-Security-Updates.md
15. 15-Time-Sync-and-SELinux-AppArmor.md
16. 16-Hands-On-Hardening-Labs.md
17. 17-Troubleshooting-Hardening-Scenarios.md
18. 18-Interview-Questions-and-Answers.md
19. 19-MCQs-and-Quick-Revision.md
20. 20-Commands-Cheat-Sheet-and-Checklist.md

`SOURCE.md` preserves the foundation and critical safety warnings for this topic.
