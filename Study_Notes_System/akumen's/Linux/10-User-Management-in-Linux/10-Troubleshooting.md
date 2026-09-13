# 10 - Troubleshooting User Management Issues

This guide details common failure modes, diagnostic commands, and root-cause remedies for user, group, and privilege management issues in Linux environments.

---

## 🔍 Issue 1: "user is not in the sudoers file. This incident will be reported."

### Symptom
When a user attempts to run `sudo systemctl restart nginx`, the terminal displays:
`alice is not in the sudoers file. This incident will be reported.`

### Root Cause
User `alice` is not listed in `/etc/sudoers` or `/etc/sudoers.d/`, nor is she a member of the system administrative group (`sudo` on Ubuntu/Debian, `wheel` on RHEL/Rocky).

### Diagnostic Steps
1. Login as root or user with sudo access.
2. Check user's current group memberships:
   ```bash
   id alice
   ```

### Resolution
* **For Debian / Ubuntu:**
  ```bash
  sudo usermod -aG sudo alice
  ```
* **For RHEL / CentOS / Rocky Linux:**
  ```bash
  sudo usermod -aG wheel alice
  ```
* *Note:* The user must **log out and log back in** (or run `su - alice` / `newgrp`) for new group credentials to take effect in the shell!

---

## 🔍 Issue 2: `visudo` Syntax Error Locks Out Sudo Privileges

### Symptom
Every execution of `sudo` yields:
`>>> /etc/sudoers: syntax error near line 25 <<<`
`sudo: parse error in /etc/sudoers near line 25`
`sudo: no valid sudoers sources found, quitting`

### Root Cause
Direct manual editing of `/etc/sudoers` (without using `visudo`) introduced a syntax error (e.g. missing colon or invalid username), blocking all sudo access.

### Resolution
1. **If Root Password is Known:**
   Switch directly to root without sudo:
   ```bash
   su -
   visudo
   ```
2. **If Root Account is Locked (Ubuntu AWS EC2 instances):**
   Reboot instance into Recovery / Single-User Mode, or attach root EBS volume to a rescue instance and repair `/etc/sudoers` using `visudo -f /mnt/rescue/etc/sudoers`.

---

## 🔍 Issue 3: `userdel: user alice is currently logged in` / `userdel: user alice is currently used by process 4512`

### Symptom
Attempting to run `userdel -r alice` fails with an error indicating processes are active or user is logged in.

### Diagnostic & Resolution
1. Find all active processes owned by the user:
   ```bash
   pgrep -u alice -l
   ```
2. Send SIGTERM (`15`) or SIGKILL (`9`) to terminate processes:
   ```bash
   sudo pkill -9 -u alice
   ```
3. Verify no open systemd services or screen/tmux sessions remain:
   ```bash
   sudo systemctl stop user@$(id -u alice).service || true
   ```
4. Retry account deletion:
   ```bash
   sudo userdel -r alice
   ```

---

## 🔍 Issue 4: `usermod -G` Removed All Existing Supplementary Groups!

### Symptom
After running `usermod -G docker alice`, user `alice` lost access to `sudo`, `devs`, and `libvirt` groups.

### Root Cause
Omitting the `-a` (append) flag in `usermod -G` overwrites the user's supplementary group list with **only** the groups specified in the command!

### Resolution
Re-add all required groups using the `-a` append flag:
```bash
sudo usermod -aG sudo,devs,libvirt,docker alice
```
> 💡 **Best Practice Rule:** ALWAYS use `-aG` together when modifying groups!
