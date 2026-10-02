# 14 - Quick Revision Cheat Sheet

High-density summary cheat sheet for fast review before interviews, exams, or operational tasks.

---

## ⚡ Core Files Summary

| File | Purpose | Permissions | Key Information |
| :--- | :--- | :--- | :--- |
| `/etc/passwd` | Account Metadata | `-rw-r--r--` (`644`) | Username, UID, Primary GID, Home Dir, Default Shell |
| `/etc/shadow` | Passwords & Aging | `-rw-r-----` (`640`/`600`) | Password Hashes (`$6$`, `$y$`), Aging parameters |
| `/etc/group` | Group Membership | `-rw-r--r--` (`644`) | Group Name, GID, Supplementary User Members |
| `/etc/gshadow` | Group Passwords | `-rw-r-----` (`640`/`600`) | Encrypted group passwords & Group Admins |

---

## 🔑 UID Breakdown
* **`0`**: Superuser / Root
* **`1 - 999`**: System Service Accounts (Shell = `/sbin/nologin`)
* **`1000 - 60000`**: Regular Interactive Human Users
* **`65534`**: `nobody` / `nogroup`

---

## 🛠️ Must-Remember Commands

```bash
# 1. User Creation
sudo useradd -m -s /bin/bash alice          # Create user with home & bash shell
sudo useradd -r -s /sbin/nologin appuser     # Create unprivileged system account

# 2. Modify Groups
sudo usermod -aG docker,sudo alice           # ALWAYS use -aG to append groups!

# 3. Password & Expiration
sudo passwd alice                            # Set password
sudo passwd -l alice                         # Lock account
sudo chage -d 0 alice                        # Force password change on next login
sudo chage -M 90 -W 7 -I 14 alice            # Set 90-day password aging policy

# 4. Sudo & Sudoers
sudo visudo                                  # Edit /etc/sudoers safely
sudo visudo -cf /etc/sudoers.d/devops        # Check syntax of drop-in file (chmod 0440)

# 5. User Removal
sudo userdel -r alice                        # Delete account + home directory
```

---

## 💡 Quick Rules of Thumb
1. **Never edit `/etc/sudoers` directly.** Always use `visudo`.
2. **Never omit `-a` with `usermod -G`.** Use `usermod -aG group user` to append secondary groups without wiping existing ones.
3. **Debian/Ubuntu admin group = `sudo`**. **RHEL/Rocky/Fedora admin group = `wheel`**.
4. Drop-in files under `/etc/sudoers.d/` must be permissions `0440` and **cannot contain dots (`.`) in filenames**.
5. Linux kernel evaluates permissions by **numerical UID/GID**, not string usernames.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - MCQ](./13-MCQ.md) | [README](./README.md) | [15 - Related Topics](./15-Related-Topics.md) |
