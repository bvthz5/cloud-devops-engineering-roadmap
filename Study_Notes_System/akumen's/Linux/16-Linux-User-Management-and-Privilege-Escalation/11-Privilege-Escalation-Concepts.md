# 11 - Privilege Escalation Concepts

Understanding how attackers escalate from a low-privilege shell to root is essential for hardening your systems. Every misconfiguration in user management or sudo rules is a potential attack vector.

---

## 📈 What is Privilege Escalation?

It is the act of exploiting a vulnerability, design flaw, or misconfiguration to gain elevated access — typically from a normal user to `root`.

There are two types:

| Type | Description | Example |
| :--- | :--- | :--- |
| **Vertical** | Gaining higher privileges (user → root). | Exploiting a SUID binary to get a root shell. |
| **Horizontal** | Gaining access to another user at the same privilege level. | Accessing `bob`'s files from `alice`'s account. |

---

## 🕳️ Common Privilege Escalation Vectors

### 1. Misconfigured `sudo` Rules
The most common vector. If a user has `NOPASSWD: ALL`, or can run a program that allows shell escapes (like `vi`, `less`, `find`, `python`), they can trivially get a root shell.

```text
# Dangerous rule:
alice   ALL=(root)   NOPASSWD: /usr/bin/vi
```
*Alice runs `sudo vi`, then types `:!bash` inside vi, and gets a root shell.*

**Mitigation:** Never grant sudo access to editors, interpreters (`python`, `perl`), or commands with built-in shell escapes. Use specific, non-interactive commands.

### 2. SUID Binaries
As covered in the Permissions module, a file with the SUID bit runs with the owner's privileges. If a custom script owned by root has SUID set and contains a vulnerability, it can be exploited.

```bash
# Find all SUID root binaries on the system:
find / -perm -4000 -user root -type f 2>/dev/null
```

**Mitigation:** Audit SUID binaries regularly. Remove the SUID bit from anything that doesn't strictly need it.

### 3. Writable `/etc/passwd` or `/etc/shadow`
If an attacker can write to `/etc/passwd`, they can add a new line with UID 0 and immediately become root.

**Mitigation:** Ensure correct permissions: `/etc/passwd` is `644` (readable by all, writable only by root), `/etc/shadow` is `640` (readable only by root and shadow group).

### 4. Kernel Exploits
If the Linux kernel itself has an unpatched vulnerability (like "Dirty COW" or "Dirty Pipe"), a local user can exploit it to gain root.

**Mitigation:** Keep the kernel patched and updated. Use `uname -r` to check the running kernel version.

### 5. Cron Jobs Running as Root
If root has a cron job that executes a script, and that script is world-writable, an attacker can inject malicious commands into it.

**Mitigation:** Ensure scripts executed by root cron jobs are owned by root and have restrictive permissions (`700` or `750`).

---

## 🔍 Checking for Common Weaknesses

```bash
# 1. Check sudo permissions for the current user
sudo -l

# 2. Find all SUID files
find / -perm -4000 -type f 2>/dev/null

# 3. Find world-writable files
find / -writable -type f 2>/dev/null

# 4. Check for extra UID 0 accounts
awk -F: '$3 == 0 {print $1}' /etc/passwd

# 5. Check cron jobs running as root
sudo crontab -l
cat /etc/crontab
ls -la /etc/cron.d/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Service Accounts and nologin](./10-Service-Accounts-and-nologin.md) | [README](./README.md) | [12 - Security Auditing](./12-Security-Auditing.md) |
