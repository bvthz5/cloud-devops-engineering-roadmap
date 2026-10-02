# 16 - Hands-On Terminal Practice

This lab provides exercises for managing users, passwords, sudo rules, and auditing.

> **Prerequisites:** A Linux terminal with `sudo` privileges. Some exercises create and delete users.

---

## 🛠️ Level 1: User Lifecycle

**Tasks:**
1. Create a new user `testdev` with a home directory, bash shell, and the comment "Test Developer".
2. Set a password for `testdev`.
3. Verify the entry in `/etc/passwd`.
4. Check the password hash type in `/etc/shadow`.

**Commands:**
```bash
# 1. Create user
sudo useradd -m -s /bin/bash -c "Test Developer" testdev

# 2. Set password
sudo passwd testdev

# 3. View /etc/passwd entry
grep testdev /etc/passwd

# 4. View /etc/shadow hash (look for $6$ = SHA-512)
sudo grep testdev /etc/shadow
```

---

## ⬛ Level 2: Groups and Membership

**Tasks:**
1. Create a group called `labteam`.
2. Add `testdev` to `labteam` as a secondary group.
3. Verify the membership.
4. Create a file as `testdev` and check its group ownership.

**Commands:**
```bash
# 1. Create group
sudo groupadd labteam

# 2. Add to group (don't forget -a!)
sudo usermod -aG labteam testdev

# 3. Verify
groups testdev
# Or: id testdev

# 4. Create file and check
sudo -u testdev touch /tmp/testfile.txt
ls -l /tmp/testfile.txt
# The group should be testdev's primary group, not labteam
```

---

## 🟫 Level 3: Password Aging

**Tasks:**
1. Force `testdev` to change their password on the next login.
2. Set the maximum password age to 60 days.
3. Set the warning period to 10 days.
4. View the human-readable password aging information.

**Commands:**
```bash
# 1. Force change on next login
sudo chage -d 0 testdev

# 2. Max age 60 days
sudo chage -M 60 testdev

# 3. Warning period 10 days
sudo chage -W 10 testdev

# 4. View info
sudo chage -l testdev
```

---

## 🟦 Level 4: sudo Configuration

**Tasks:**
1. Create a sudoers drop-in file for `testdev` using `visudo`.
2. Allow `testdev` to run only `/usr/bin/systemctl status` as root, without a password.
3. Test it by running `sudo systemctl status sshd` as `testdev`.
4. Verify that other sudo commands are denied.

**Commands:**
```bash
# 1. Create drop-in file
sudo visudo -f /etc/sudoers.d/testdev

# 2. Content:
# testdev   ALL=(root)   NOPASSWD: /usr/bin/systemctl status *

# 3. Test (as testdev)
sudo -u testdev sudo systemctl status sshd

# 4. This should FAIL:
sudo -u testdev sudo apt update
```

---

## 🟥 Level 5: Security Audit

**Tasks:**
1. Find all accounts with UID 0.
2. Find all SUID root binaries on the system.
3. Check the lock status of `testdev`.
4. Lock `testdev`, verify the lock, then unlock them.

**Commands:**
```bash
# 1. UID 0 accounts
awk -F: '$3 == 0 {print $1}' /etc/passwd

# 2. SUID root binaries
sudo find / -perm -4000 -user root -type f 2>/dev/null | head -20

# 3. Check status
sudo passwd -S testdev

# 4. Lock, verify, unlock
sudo passwd -l testdev
sudo passwd -S testdev  # Should show 'L'
sudo passwd -u testdev
sudo passwd -S testdev  # Should show 'P'
```

---

## 🧹 Cleanup

```bash
sudo userdel -r testdev
sudo groupdel labteam
sudo rm -f /etc/sudoers.d/testdev
sudo rm -f /tmp/testfile.txt
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [15 - Interview QA](./15-Interview-QA.md) | [README](./README.md) | [17 - MCQs](./17-MCQs.md) |
