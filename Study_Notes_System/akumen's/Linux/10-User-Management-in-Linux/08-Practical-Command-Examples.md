# 08 - Practical Command Examples

This cheat sheet provides copy-paste ready CLI commands for day-to-day Linux user management, group administration, account locking, and audit verification.

---

## 📋 Comprehensive Command Reference Table

| Action / Goal | Command |
| :--- | :--- |
| **Create user with home dir & bash shell** | `sudo useradd -m -s /bin/bash john` |
| **Create system service account** | `sudo useradd -r -s /sbin/nologin -d /var/lib/app appuser` |
| **Set/Change user password** | `sudo passwd john` |
| **Set password non-interactively in script** | `echo "john:Password123!" | sudo chpasswd` |
| **Add user to supplementary group** | `sudo usermod -aG docker john` |
| **Change user's primary group** | `sudo usermod -g developers john` |
| **Lock user account** | `sudo passwd -l john` *or* `sudo usermod -L john` |
| **Unlock user account** | `sudo passwd -u john` *or* `sudo usermod -U john` |
| **Force user to change password on login** | `sudo chage -d 0 john` |
| **Set password expiration policy (90 days)**| `sudo chage -M 90 -W 7 -I 14 john` |
| **Inspect user password aging details** | `sudo chage -l john` |
| **Delete user (keep home directory)** | `sudo userdel john` |
| **Delete user AND remove home directory** | `sudo userdel -r john` |
| **Create a new group** | `sudo groupadd developers` |
| **Add user to group using `gpasswd`** | `sudo gpasswd -a john developers` |
| **Remove user from group using `gpasswd`**| `sudo gpasswd -d john developers` |
| **Check current logged-in user credentials** | `id` |
| **Check group membership for another user** | `groups john` |
| **List logged-in users** | `who` *or* `w` |
| **Validate `/etc/sudoers` syntax** | `sudo visudo -c` |
| **Check current user's sudo privileges** | `sudo -l` |

---

## 🛠️ Real-World Administration Workflows

### 1. New Developer Onboarding Script Snippet
```bash
#!/usr/bin/env bash
set -euo pipefail

USERNAME="developer1"
EMAIL="dev1@company.com"

# Create user with primary group 'developers' and supplementary group 'docker'
sudo groupadd -f developers
sudo useradd -m -g developers -G docker -s /bin/bash -c "$EMAIL" "$USERNAME"

# Set temporary password and force change on initial SSH login
echo "$USERNAME:TempPass2026!" | sudo chpasswd
sudo chage -d 0 "$USERNAME"

echo "User $USERNAME successfully onboarded."
```

### 2. Immediate Employee Offboarding Script Snippet
```bash
#!/usr/bin/env bash
set -euo pipefail

TARGET_USER="former_employee"

echo "Offboarding user: $TARGET_USER"

# 1. Lock password in /etc/shadow
sudo passwd -l "$TARGET_USER"

# 2. Terminate all active processes owned by user
sudo pkill -9 -u "$TARGET_USER" || true

# 3. Disable login shell
sudo usermod -s /sbin/nologin "$TARGET_USER"

# 4. Backup home directory before deletion
sudo tar -czf "/backups/${TARGET_USER}_home_$(date +%F).tar.gz" "/home/${TARGET_USER}" || true

# 5. Delete account and home directory
sudo userdel -r "$TARGET_USER"

echo "Offboarding complete for $TARGET_USER."
```
