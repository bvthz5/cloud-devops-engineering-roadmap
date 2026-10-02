# 4. Password Policies and PAM

## Pluggable Authentication Modules (PAM)
PAM handles user authentication policies across Linux services.

## Password Quality Enforcement (`pam_pwquality`)

Install password quality module (Debian/Ubuntu):
```bash
sudo apt install libpam-pwquality -y
```

Configure `/etc/security/pwquality.conf`:
```ini
# Minimum password length
minlen = 14

# Require at least one uppercase letter
ucredit = -1

# Require at least one lowercase letter
lcredit = -1

# Require at least one digit
dcredit = -1

# Require at least one special character
ocredit = -1

# Reject passwords containing user's username
usercheck = 1
```

## Password Expiration & Aging (`/etc/login.defs`)
Set default aging policies for new users:
```ini
PASS_MAX_DAYS   90
PASS_MIN_DAYS   7
PASS_WARN_AGE   14
```

Apply to existing users:
```bash
sudo chage -M 90 -m 7 -W 14 username
```

## Account Lockout Policy (`pam_faillock`)
Lock account after 5 consecutive failed login attempts:
Add to `/etc/pam.d/common-auth` or `/etc/pam.d/system-auth`:
```ini
auth required pam_faillock.so preauth silent audit deny=5 unlock_time=900
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - User and Account Security](./03-User-and-Account-Security.md) | [README](./README.md) | [05 - SSH Hardening](./05-SSH-Hardening.md) |
