# 09 - Password Management and Aging

Passwords are only one layer of authentication, but they remain the default on most Linux systems. Managing password lifecycle — expiration, complexity, and forced rotation — is critical for compliance and security.

---

## 🔄 `chage` — Change Age

`chage` is the primary tool for viewing and configuring password aging policies stored in `/etc/shadow`.

### Viewing Password Info
```bash
sudo chage -l alice
```

### Setting Maximum Password Age
Force the user to change their password every 90 days:
```bash
sudo chage -M 90 alice
```

### Setting Minimum Password Age
Prevent the user from changing their password for at least 7 days (prevents rapid password cycling to reuse old passwords):
```bash
sudo chage -m 7 alice
```

### Setting Warning Days
Warn the user 14 days before their password expires:
```bash
sudo chage -W 14 alice
```

### Setting Account Expiration Date
Disable the account entirely after a specific date (e.g., for a contractor):
```bash
sudo chage -E 2027-03-31 alice
```

### Force Password Change on Next Login
Commonly used when creating new accounts or after a security incident:
```bash
sudo chage -d 0 alice
```
*This sets the "last changed" date to the Unix Epoch (Day 0), making the system believe the password is infinitely old, forcing an immediate change.*

---

## 🔒 Locking and Unlocking Accounts

### Locking (Disabling Password Login)
Locking an account prepends `!` to the password hash in `/etc/shadow`, making it impossible to authenticate.

```bash
# Using passwd
sudo passwd -l alice

# Using usermod
sudo usermod -L alice
```
*Note: Locking only disables password-based login. If the user has SSH key-based authentication configured, they can still log in. To fully disable an account, also set the shell to `/sbin/nologin` or set an expiration date.*

### Unlocking
```bash
sudo passwd -u alice
sudo usermod -U alice
```

### Checking Lock Status
```bash
sudo passwd -S alice
```
*Output: `alice L 2026-05-10 0 99999 7 -1` — The `L` means Locked. `P` means Password set.*

---

## 🗓️ DevOps Best Practices for Password Aging

| Policy | Recommended Value | Rationale |
| :--- | :--- | :--- |
| **Maximum Age** | 90 days (or never if using MFA/SSH keys) | Balance between security and usability. |
| **Minimum Age** | 1–7 days | Prevents cycling through password history. |
| **Warning Period** | 7–14 days | Gives users time to change proactively. |
| **Inactivity Lockout** | 30 days | Auto-disables accounts that ignore expiry. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - NOPASSWD and Least Privilege](./08-NOPASSWD-and-Least-Privilege.md) | [README](./README.md) | [10 - Service Accounts and nologin](./10-Service-Accounts-and-nologin.md) |
