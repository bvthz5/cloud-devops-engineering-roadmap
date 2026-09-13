# 12 - Security Auditing

Proactive auditing is the difference between a secure system and an incident waiting to happen. Run these audits regularly (monthly, or automated via cron).

---

## 🔍 Audit 1: Unauthorized UID 0 Accounts

Any account with UID 0 has unrestricted root access.

```bash
awk -F: '$3 == 0 {print $1}' /etc/passwd
```
**Expected output:** Only `root`. Anything else is a critical finding.

---

## 🔍 Audit 2: Accounts With Empty Passwords

An empty password field in `/etc/shadow` allows login without any authentication.

```bash
sudo awk -F: '($2 == "" || $2 == "!") {print $1}' /etc/shadow
```
**Action:** Lock any account with an empty password immediately: `sudo passwd -l <username>`.

---

## 🔍 Audit 3: Stale / Inactive Accounts

Former employees, contractors, and test accounts that are never cleaned up are prime targets for attackers.

```bash
# Find users who have never logged in (last login)
lastlog | grep "Never logged in"

# Find users whose passwords haven't been changed in over 180 days
sudo awk -F: '{if ($3 != "" && $3 < (systime()/86400 - 180)) print $1}' /etc/shadow
```
**Action:** Lock or delete stale accounts.

---

## 🔍 Audit 4: SUID/SGID Binaries

Every SUID/SGID binary is a potential privilege escalation vector.

```bash
# Find all SUID files
find / -perm -4000 -type f 2>/dev/null

# Find all SGID files
find / -perm -2000 -type f 2>/dev/null
```
**Action:** Compare against a known-good baseline. Investigate any unfamiliar SUID binaries.

---

## 🔍 Audit 5: Sudo Privileges

Review who has sudo access and what they can do.

```bash
# Check sudoers and all drop-in files
sudo cat /etc/sudoers
sudo ls -la /etc/sudoers.d/
sudo cat /etc/sudoers.d/*
```
**Action:** Ensure no user has `NOPASSWD: ALL` unless they are a verified automation service account. Remove rules for users who no longer need them.

---

## 🔍 Audit 6: World-Writable Files in Sensitive Locations

If a script run by root (via cron or systemd) is world-writable, any user can inject malicious code into it.

```bash
find /etc /usr /var -writable -type f 2>/dev/null
```
**Action:** Remove world-writable permissions from any sensitive files.

---

## 📋 One-Shot Audit Script

Combine the above into a single script:

```bash
#!/bin/bash
echo "=== UID 0 Accounts ==="
awk -F: '$3 == 0 {print $1}' /etc/passwd

echo -e "\n=== Empty Password Accounts ==="
awk -F: '($2 == "" || $2 == "!") {print $1}' /etc/shadow

echo -e "\n=== SUID Root Binaries ==="
find / -perm -4000 -user root -type f 2>/dev/null

echo -e "\n=== World-Writable in /etc ==="
find /etc -writable -type f 2>/dev/null

echo -e "\n=== Sudoers Drop-in Files ==="
ls -la /etc/sudoers.d/
```
