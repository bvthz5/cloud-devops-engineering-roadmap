# 06 — Linux Security Auditing with auditd

The Linux Audit Daemon (`auditd`) collects security-relevant event information directly from the kernel subsystem. Unlike syslog, auditd records can track who modified a sensitive file, who executed a command, and which system calls failed.

---

## 1. Creating Audit Rules (`/etc/audit/rules.d/audit.rules`)

```text
# 1. Watch critical system configuration files for modifications
-w /etc/passwd -p wa -k identity_changes
-w /etc/shadow -p wa -k identity_changes
-w /etc/sudoers -p wa -k sudo_changes

# 2. Track all executions of the 'sudo' or 'su' binaries
-a always,exit -F path=/usr/bin/sudo -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged_exec
```
- `-w <path>`: Watch this file or directory.
- `-p wa`: Trigger on Write (`w`) or Attribute changes (`a`).
- `-k <key>`: Custom search tag to query later.

---

## 2. Searching Audit Logs with `ausearch` and `aureport`

```bash
# Search for events tagged with our 'identity_changes' key
sudo ausearch -k identity_changes

# Generate an executive summary report of authentication attempts
sudo aureport -au

# Generate a report of failed system calls (detect exploitation attempts)
sudo aureport -s --failed
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Centralized Log Aggregation Shippers](./05-Centralized-Log-Aggregation-Shippers.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
