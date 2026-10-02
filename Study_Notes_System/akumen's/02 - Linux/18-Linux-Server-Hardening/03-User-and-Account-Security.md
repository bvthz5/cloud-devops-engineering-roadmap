# 3. User and Account Security

## Securing User Accounts

### 1. Disable Direct Root Login
Root execution should occur via `sudo` from an authorized user account.

Lock the root user password:
```bash
sudo passwd -l root
```

### 2. Audit System Accounts
Inspect `/etc/passwd` to ensure non-human system accounts (e.g., `www-data`, `nobody`, `daemon`) are assigned non-login shells:
```bash
# Set shell to /usr/sbin/nologin
sudo usermod -s /usr/sbin/nologin www-data
```

### 3. Lock or Remove Inactive Users
```bash
# Lock a user account
sudo usermod -L username

# Expire a user account immediately
sudo chage -E 0 username

# Delete an obsolete user and their home directory
sudo userdel -r olduser
```

### 4. Inspect Empty Passwords & UID 0 Accounts
Only `root` should have `UID 0`.
```bash
# Find any non-root account with UID 0
awk -F: '($3 == "0") { print $1 }' /etc/passwd

# Check for accounts with empty passwords in /etc/shadow
sudo awk -F: '($2 == "") { print $1 }' /etc/shadow
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Patch Management](./02-Patch-Management.md) | [README](./README.md) | [04 - Password Policies and PAM](./04-Password-Policies-and-PAM.md) |
