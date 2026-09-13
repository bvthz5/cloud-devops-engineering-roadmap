# 18 - Quick Revision Cheat Sheet

High-density 5-minute summary for fast review before interviews or on-call auditing.

---

## 👤 User Categories

| Category | UID | Example |
| :--- | :---: | :--- |
| Root | `0` | `root` |
| System/Service | `1–999` | `www-data`, `nginx` |
| Regular | `1000+` | `alice`, `deploy` |

---

## 📁 The Three Identity Files

| File | Purpose | Permissions |
| :--- | :--- | :--- |
| `/etc/passwd` | 7 fields: `user:x:UID:GID:GECOS:home:shell` | `644` (world-readable) |
| `/etc/shadow` | 9 fields: `user:hash:lastchg:min:max:warn:inactive:expire:` | `640` (root only) |
| `/etc/group` | Group → GID → members | `644` |

**Hash Prefixes:** `$1$`=MD5 ❌, `$5$`=SHA-256 ✅, `$6$`=SHA-512 ✅✅, `$y$`=yescrypt ✅✅✅

---

## 🛠️ User Management One-Liners

```text
sudo useradd -m -s /bin/bash -G sudo,docker alice  → Create user
sudo passwd alice                                    → Set password
sudo usermod -aG devteam alice                       → Add to group (MUST use -a!)
sudo userdel -r alice                                → Delete user + home
sudo chage -d 0 alice                                → Force password change on next login
sudo chage -E 2027-12-31 alice                       → Set account expiration
sudo passwd -l alice                                 → Lock account
sudo passwd -u alice                                 → Unlock account
```

---

## 🔑 sudo Cheat Sheet

| Command | Purpose |
| :--- | :--- |
| `sudo <cmd>` | Run as root (prompts for YOUR password). |
| `sudo -l` | List your sudo privileges. |
| `sudo -i` | Full root login shell (clean environment). |
| `sudo -s` | Root shell (keeps your environment). |
| `sudo -u postgres psql` | Run as a specific user. |

---

## 📐 Sudoers Syntax

```text
WHO    WHERE = (AS_WHOM)    WHAT

alice   ALL  = (ALL:ALL)     ALL                      → Full root
%devops ALL  = (ALL:ALL)     ALL                      → Group rule
deploy  ALL  = (root)  NOPASSWD: /usr/bin/systemctl   → Scoped, passwordless
```

**Rules:**
*   Always edit with `visudo` (validates syntax).
*   Use modular files: `visudo -f /etc/sudoers.d/deploy`.
*   No dots or tildes in drop-in filenames.

---

## 🛡️ Security Audits

```text
awk -F: '$3 == 0 {print $1}' /etc/passwd      → UID 0 accounts (should be only root)
find / -perm -4000 -type f 2>/dev/null          → SUID binaries
sudo cat /etc/sudoers.d/*                       → Review sudo drop-ins
lastlog | grep "Never logged in"                → Stale accounts
```

---

## 🎯 30-Second Interview Answer

> *"Linux user management revolves around `/etc/passwd` (identity), `/etc/shadow` (password hashes and aging), and `/etc/group` (memberships). Privilege escalation is controlled via `sudo` and `/etc/sudoers`, which must always be edited with `visudo` to prevent syntax errors from locking out administrators. Best practices include using modular drop-in files in `/etc/sudoers.d/`, applying least-privilege by scoping sudo rules to specific commands, reserving `NOPASSWD` for non-interactive service accounts, creating service accounts with `/sbin/nologin`, and regularly auditing for unauthorized UID 0 accounts and excessive SUID binaries."*
