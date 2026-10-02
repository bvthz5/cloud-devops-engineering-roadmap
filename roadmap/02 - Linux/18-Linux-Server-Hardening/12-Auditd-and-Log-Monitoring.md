# 12. Auditd and Log Monitoring

## Linux Audit Framework (`auditd`)
`auditd` tracks security-relevant system events, file access, and kernel calls.

## Installation & Setup
```bash
sudo apt install auditd audispd-plugins -y   # Debian/Ubuntu
sudo dnf install audit -y                     # RHEL/CentOS
sudo systemctl enable --now auditd
```

## Defining Audit Rules (`/etc/audit/rules.d/audit.rules`)
```ini
# Monitor changes to /etc/passwd and /etc/shadow
-w /etc/passwd -p wa -k user_modification
-w /etc/shadow -p wa -k user_modification
-w /etc/sudoers -p wa -k sudoers_changes

# Track execution of privilege escalation commands
-a always,exit -F arch=b64 -S execve -F euid=0 -k root_commands
```

## Searching Audit Logs
```bash
# Search by rule key
sudo ausearch -k user_modification

# Generate audit summary report
sudo aureport --summary
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Kernel Hardening with Sysctl](./11-Kernel-Hardening-with-Sysctl.md) | [README](./README.md) | [13 - File Integrity AIDE Rootkit Checkers](./13-File-Integrity-AIDE-Rootkit-Checkers.md) |
