# 14 - Troubleshooting

---

## 🟥 "alice is not in the sudoers file. This incident will be reported."

**The Cause:** The user is not in the `sudo` or `wheel` group, and has no entry in `/etc/sudoers` or `/etc/sudoers.d/`.

**The Fix (if you have root access or another sudo user):**
```bash
# Add alice to the sudo group (Debian/Ubuntu)
sudo usermod -aG sudo alice

# Or add to the wheel group (RHEL/CentOS)
sudo usermod -aG wheel alice

# Or create a dedicated sudoers drop-in
sudo visudo -f /etc/sudoers.d/alice
# Content: alice   ALL=(ALL:ALL)   ALL
```
*Alice must log out and log back in for the group change to take effect, or run `newgrp sudo`.*

---

## 🟧 "Account is locked" / "Authentication token manipulation error"

**The Cause:** The user's account has been locked (either deliberately or by too many failed password attempts if `pam_tally2` or `pam_faillock` is configured).

**The Fix:**
```bash
# Check lock status
sudo passwd -S alice
# 'L' = Locked, 'P' = Password set

# Unlock the account
sudo passwd -u alice
# Or: sudo usermod -U alice
```

---

## 🟨 User Can SSH via Key but Not via Password

**The Cause:** The user's password is either not set (`!!` in `/etc/shadow`) or is expired.

**The Fix:**
```bash
# Check password status
sudo passwd -S alice

# Set a new password
sudo passwd alice

# Check if it's expired
sudo chage -l alice
```

---

## 🟩 `visudo` Reports a Syntax Error

**The Cause:** You (or an automation tool) introduced a bad line into `/etc/sudoers` or a drop-in file.

**The Fix:**
If `visudo` detects a syntax error upon saving:
1.  It will prompt you: `What now?`
2.  Press `e` to re-edit and fix the error.
3.  Press `x` to exit **without saving** (safest).
4.  **Never press `Q` (save anyway)** — this will break `sudo` for everyone.

If `sudo` is already broken:
```bash
# If you have direct root access (e.g., console or recovery mode):
pkexec visudo
# Or boot into single-user/recovery mode and fix /etc/sudoers directly.
```

---

## 🟦 New Group Membership Not Taking Effect

**The Cause:** Linux reads group memberships at login time. Adding a user to a group while they are logged in does not take effect until the next login.

**The Fix:**
```bash
# Option 1: Log out and back in (best)
exit
# Then SSH back in.

# Option 2: Use newgrp to activate a specific group in the current session
newgrp docker

# Option 3: Replace the current shell session
exec su -l $(whoami)
```
