# 15 - Interview Questions and Answers

User management and privilege escalation are core topics in any Linux security, SysAdmin, or DevOps interview.

---

## 🟢 Fundamentals

**Q1. What are the three types of users in Linux?**
> Root (UID 0), System/Service users (UID 1–999), and Regular users (UID 1000+).

---

**Q2. What are the 7 fields in `/etc/passwd`?**
> Username, Password placeholder (`x`), UID, GID, GECOS/Comment, Home directory, Login shell.

---

**Q3. Why does `/etc/passwd` not store actual passwords?**
> Historically it did, but `/etc/passwd` must be world-readable (for UID-to-name lookups). Storing password hashes in a world-readable file is a severe security risk. They were moved to `/etc/shadow`, which is readable only by root.

---

**Q4. What does an `!` or `!!` at the beginning of the password hash in `/etc/shadow` mean?**
> `!` means the account's password is locked. `!!` means a password has never been set. In both cases, password-based login is impossible.

---

## 🔵 sudo and Privilege Escalation

**Q5. What is the difference between `sudo -i` and `sudo -s`?**
> `sudo -i` opens a full login shell as root, loading root's environment (`.bashrc`, `.profile`). `sudo -s` opens a root shell but keeps the current user's environment variables. `sudo -i` is cleaner and preferred.

---

**Q6. Why should you ALWAYS use `visudo` to edit `/etc/sudoers`?**
> `visudo` locks the file to prevent concurrent edits and, critically, validates the syntax before saving. A syntax error in `/etc/sudoers` will break `sudo` for all users, potentially locking every administrator out of root access.

---

**Q7. What is `NOPASSWD` in sudoers, and when is it appropriate?**
> `NOPASSWD` allows a user to run sudo commands without entering their password. It is appropriate for non-interactive service accounts (CI/CD runners, Ansible, monitoring agents) that cannot type passwords. It should be scoped to specific commands, not `ALL`.

---

**Q8. Explain the sudoers rule: `deploy ALL=(root) NOPASSWD: /usr/bin/systemctl restart nginx`**
> The user `deploy` on `ALL` hosts can run `/usr/bin/systemctl restart nginx` as the `root` user `NOPASSWD` (without entering a password). They cannot run any other command via sudo.

---

## 🟠 Advanced Security

**Q9. What is privilege escalation, and how would you prevent it?**
> Privilege escalation is gaining higher permissions than intended (e.g., user → root). Prevention includes: using least-privilege sudo rules, auditing SUID binaries, keeping the kernel patched, never granting sudo to editors or interpreters, using `/sbin/nologin` for service accounts, and auditing for unauthorized UID 0 accounts.

---

**Q10. How do you check if there are unauthorized root-level accounts?**
> `awk -F: '$3 == 0 {print $1}' /etc/passwd` — This should only return `root`.

---

**Q11. A contractor's access needs to expire automatically on December 31st. How do you configure this?**
> `sudo chage -E 2026-12-31 contractor_name` — This sets the absolute account expiration date. After this date, the user cannot log in.

---

**Q12. What is the difference between locking an account with `passwd -l` and setting the shell to `/sbin/nologin`?**
> `passwd -l` only disables password-based authentication. The user can still log in with SSH keys. Setting the shell to `/sbin/nologin` prevents all interactive login, including SSH key-based access. For a complete lockout, do both.

---

## 💡 30-Second Interview Answer

> *"Linux user management centers on three files: `/etc/passwd` (identity mapping), `/etc/shadow` (password hashes and aging), and `/etc/group` (group memberships). Privilege escalation is controlled via `sudo` and the `/etc/sudoers` file, which must always be edited using `visudo` for syntax validation. Best practices include using modular drop-in files in `/etc/sudoers.d/`, applying the principle of least privilege (scoping sudo rules to specific commands), using `NOPASSWD` only for non-interactive service accounts, creating service accounts with `/sbin/nologin`, and regularly auditing for unauthorized UID 0 accounts, stale users, and excessive SUID binaries."*
