# 07 - Special Permissions: SUID, SGID, and Sticky Bit

Beyond standard read, write, and execute permissions, Linux offers three "special" permissions that handle edge cases involving security, execution contexts, and shared directories. 

When configuring special permissions numerically, a **fourth digit** is prepended to the standard 3-digit octal code (e.g., `chmod 4755`).

---

## 🦸‍♂️ 1. Set User ID (SUID / setuid)

**Applies to:** Executable files only.

**What it does:** When an executable with SUID is run, it executes with the privileges of the **file's owner**, NOT the privileges of the user running it.

**Why it's needed:** The classic example is `/usr/bin/passwd`. Normal users need to change their passwords, which requires writing to `/etc/shadow`. However, only `root` has write access to `/etc/shadow`. Because `/usr/bin/passwd` is owned by `root` and has the SUID bit set, when a normal user runs it, the program temporarily gains `root` powers just for that execution.

**How to set it:**
*   **Symbolic:** `chmod u+s /path/to/executable`
*   **Octal (prepend 4):** `chmod 4755 /path/to/executable`

**How it looks in `ls -l`:**
The user's execute (`x`) is replaced by an `s`.
```text
-rwsr-xr-x 1 root root 68208 May 28  2020 /usr/bin/passwd
```
*(If the user lacked `x` originally, it appears as a capital `S`, indicating a broken/useless SUID).*

---

## 👥 2. Set Group ID (SGID / setgid)

**Applies to:** Executable files OR Directories.

**What it does on Files:** Similar to SUID, but the file executes with the privileges of the **file's group** owner.
**What it does on Directories (More Common):** When a file is created inside a directory with SGID set, the new file inherits the group ownership of the directory, rather than the primary group of the user who created it.

**Why it's needed:** Collaborative directories. If a team of developers (group `webdev`) shares `/var/www/html`, setting SGID on `/var/www/html` ensures that any file created by *any* developer is automatically owned by the `webdev` group, allowing other team members to edit it.

**How to set it:**
*   **Symbolic:** `chmod g+s /path/to/dir`
*   **Octal (prepend 2):** `chmod 2775 /path/to/dir`

**How it looks in `ls -l`:**
The group's execute (`x`) is replaced by an `s`.
```text
drwxrwsr-x 2 root webdev 4096 Aug 10 12:00 /var/www/html
```

---

## 🍯 3. The Sticky Bit

**Applies to:** Directories only.

**What it does:** It prevents users from deleting or renaming files in a directory unless they are the owner of the file (or the owner of the directory, or root).

**Why it's needed:** Shared scratch spaces, most notably `/tmp`. The `/tmp` directory must be writable by everyone (`chmod 777`). Without the Sticky Bit, User A could malicious delete a temporary file created by User B, because User A has write access to the parent directory.

**How to set it:**
*   **Symbolic:** `chmod +t /path/to/dir`
*   **Octal (prepend 1):** `chmod 1777 /path/to/dir`

**How it looks in `ls -l`:**
The others' execute (`x`) is replaced by a `t`.
```text
drwxrwxrwt 14 root root 4096 Aug 10 12:34 /tmp
```
*(If others lacked `x` originally, it appears as a capital `T`).*

---

## ⚠️ Security Warning

**SUID and SGID on files are highly dangerous if misconfigured.** If an attacker finds a binary with SUID `root` that has a vulnerability, they can exploit it to execute arbitrary commands as `root`, achieving instant privilege escalation. 

Always audit SUID/SGID files regularly using `find`:
```bash
find / -perm -4000 -type f 2>/dev/null  # Find SUID files
find / -perm -2000 -type f 2>/dev/null  # Find SGID files
```
