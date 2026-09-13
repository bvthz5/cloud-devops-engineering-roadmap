# 12 - Hands-On Terminal Lab

Practice user and group management operations in a safe Linux terminal sandbox or virtual machine environment.

---

## 🎯 Lab Objectives
1. Create primary and secondary group structures.
2. Create user accounts with custom UIDs, shells, and home directories.
3. Manage password aging policies using `chage`.
4. Configure granular privilege escalation using `visudo` and `/etc/sudoers.d/`.
5. Perform secure user account offboarding.

---

## 🧪 Step-by-Step Lab Tasks

### Task 1: Create Group Hierarchy
Create two Linux groups:
* `sysadmins` with GID `2001`
* `developers` with GID `2002`

```bash
# Solution Verification Command
sudo groupadd -g 2001 sysadmins
sudo groupadd -g 2002 developers
grep -E 'sysadmins|developers' /etc/group
```

---

### Task 2: Create User Accounts
Create two user accounts:
1. User `alice`:
   * Primary Group: `developers`
   * Supplementary Group: `sudo` (or `wheel` on RHEL)
   * Shell: `/bin/bash`
   * Home Directory: `/home/alice`
2. User `appmonitor`:
   * System Account (UID < 1000)
   * Shell: `/sbin/nologin`
   * Home Directory: `/var/lib/appmonitor`

```bash
# Solution Verification Command
sudo useradd -m -g developers -G sudo -s /bin/bash alice
sudo useradd -r -s /sbin/nologin -d /var/lib/appmonitor appmonitor

id alice
id appmonitor
```

---

### Task 3: Enforce Password Aging
Configure user `alice`'s password aging parameters:
* Maximum password age: 60 days
* Expiration warning: 10 days
* Inactivity period: 7 days
* Force password update on first login.

```bash
# Solution Verification Command
sudo chage -M 60 -W 10 -I 7 alice
sudo chage -d 0 alice
sudo chage -l alice
```

---

### Task 4: Configure Passwordless Sudo Rule
Create a drop-in sudoers file `/etc/sudoers.d/developers` allowing all members of group `developers` to run `/usr/bin/systemctl status` and `/usr/bin/systemctl restart` on service `nginx` without entering a password.

```bash
# Solution Verification Command
sudo visudo -c -f /etc/sudoers.d/developers << 'EOF'
%developers ALL=(ALL) NOPASSWD: /usr/bin/systemctl status nginx, /usr/bin/systemctl restart nginx
EOF

sudo chmod 0440 /etc/sudoers.d/developers
sudo visudo -c
```

---

### Task 5: User Offboarding & Cleanup
Safely decommission account `alice`:
1. Lock password.
2. Kill any running processes (if any).
3. Backup home directory to `/tmp/alice_backup.tar.gz`.
4. Remove account and home directory recursively.

```bash
# Solution Verification Command
sudo passwd -l alice
sudo pkill -u alice || true
sudo tar -czf /tmp/alice_backup.tar.gz /home/alice
sudo userdel -r alice
id alice  # Should return: id: 'alice': no such user
```
