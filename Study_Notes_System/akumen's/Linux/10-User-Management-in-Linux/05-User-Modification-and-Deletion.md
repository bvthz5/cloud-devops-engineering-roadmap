# 05 - User Modification and Deletion

Modifying existing user attributes and safely removing accounts are essential tasks for system maintenance and employee offboarding.

---

## 🛠️ User Modification (`usermod`)

The `usermod` utility modifies user account parameters directly in `/etc/passwd`, `/etc/shadow`, and `/etc/group`.

### Syntax

```bash
usermod [options] username
```

### Essential `usermod` Flags

| Option Flag | Description & Action |
| :--- | :--- |
| **`-aG group1,group2`** | **Append** user to supplementary groups. *(⚠️ CRITICAL: Always use `-a` with `-G`! Omitting `-a` replaces existing groups!)* |
| `-g group` | Change user's **primary group**. |
| `-d /new/path` | Change home directory path in `/etc/passwd`. |
| **`-d /new/path -m`** | Change home directory path **AND move existing files** to new location. |
| `-s /bin/shell` | Change default login shell. |
| `-l newname` | Change username (login name). Home directory is not moved automatically. |
| `-u NewUID` | Change numeric UID. |
| `-L` | **Lock** user password (`!` prepended to hash in `/etc/shadow`). |
| `-U` | **Unlock** user password. |
| `-e YYYY-MM-DD` | Set absolute account expiration date. |

### Practical Examples

1. **Add user `alice` to `docker` and `sudo` groups safely:**
   ```bash
   sudo usermod -aG docker,sudo alice
   ```

2. **Move home directory to `/data/home/alice` and transfer all contents:**
   ```bash
   sudo usermod -d /data/home/alice -m alice
   ```

3. **Disable interactive login shell for offboarded user:**
   ```bash
   sudo usermod -s /sbin/nologin alice
   ```

4. **Lock account during investigation:**
   ```bash
   sudo usermod -L alice
   ```

---

## 🗑️ User Deletion (`userdel`)

The `userdel` utility removes a user account entry from `/etc/passwd`, `/etc/shadow`, and `/etc/group`.

### Syntax

```bash
userdel [options] username
```

### Critical Deletion Options

| Command | Action & Risk Level |
| :--- | :--- |
| `sudo userdel alice` | Deletes user from `/etc/passwd` & `/etc/shadow`. **Leaves home directory `/home/alice` and mail spool intact on disk.** |
| `sudo userdel -r alice` | Deletes user account **AND recursively removes home directory (`/home/alice`) and mail spool.** |
| `sudo userdel -f alice` | **Force** deletion even if user is currently logged in or owns running processes. |

---

## ⚠️ Pre-Offboarding Safety Checklist (Production Best Practices)

Before executing `userdel -r` on production systems:

1. **Kill all active running processes owned by the user:**
   ```bash
   sudo pkill -u alice
   ```
2. **Identify files owned by user outside home directory:**
   ```bash
   sudo find / -user alice -ls
   ```
3. **Backup home directory before purging:**
   ```bash
   sudo tar -czvf /backups/alice_home_$(date +%F).tar.gz /home/alice
   ```
4. **Execute account deletion:**
   ```bash
   sudo userdel -r alice
   ```
