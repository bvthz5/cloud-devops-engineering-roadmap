# 04 - Password Management and Password Aging

Password security and lifecycle enforcement in Linux rely on the `passwd` and `chage` CLI utilities working against `/etc/shadow`.

---

## 🔒 Password Management (`passwd`)

The `passwd` utility allows users to update their own password or administrators to update/lock any account's password.

### Key `passwd` Command Usages

| Command | Action |
| :--- | :--- |
| `passwd` | Changes current user's password interactively. |
| `sudo passwd alice` | Changes user `alice`'s password (root does not need old password). |
| `sudo passwd -l alice` | **Lock** user `alice`'s account (prepends `!` or `!*` to shadow hash). |
| `sudo passwd -u alice` | **Unlock** user `alice`'s account. |
| `sudo passwd -d alice` | **Delete** user password (allows passwordless login - high security risk!). |
| `sudo passwd -e alice` | **Expire** user password immediately (forces password change on next login). |
| `sudo passwd -S alice` | Display password status summary for user `alice`. |

### Non-Interactive Password Setting (Automated Scripts)

To set a user password non-interactively in shell scripts or Ansible:
```bash
echo "alice:SuperSecret123!" | sudo chpasswd
```

---

## ⏳ Password Aging Administration (`chage`)

The `chage` (Change Age) command modifies password expiration parameters stored in `/etc/shadow`.

### `chage` Command Options

```bash
chage [options] username
```

| Option | Description |
| :--- | :--- |
| `-l` | **List** current password aging settings for the user. |
| `-m` | Set **Minimum** days between password changes. |
| `-M` | Set **Maximum** days password remains valid. |
| `-W` | Set **Warning** days prior to password expiration. |
| `-I` | Set **Inactivity** days after password expiration before account locking. |
| `-E YYYY-MM-DD` | Set absolute **Account Expiration** date. |
| `-d 0` | Force immediate password change on next login (sets lastchange field to `0`). |

### Practical Examples

1. **View password expiration details for `alice`:**
   ```bash
   sudo chage -l alice
   ```
   *Output:*
   ```text
   Last password change                                    : Sep 10, 2026
   Password expires                                        : Dec 09, 2026
   Password inactive                                       : Dec 23, 2026
   Account expires                                         : never
   Minimum number of days between password change          : 0
   Maximum number of days between password change          : 90
   Number of days of warning before password expires       : 7
   ```

2. **Enforce 90-day password expiration policy with 7-day warning:**
   ```bash
   sudo chage -M 90 -W 7 -I 14 alice
   ```

3. **Force user to set new password upon first login:**
   ```bash
   sudo chage -d 0 alice
   ```

---

## 🔐 Encrypted Hash Identifiers in `/etc/shadow`

The password field in `/etc/shadow` starts with a prefix between dollar signs (`$id$`) indicating the cryptographic hash function used:

| Prefix | Cryptographic Hash Algorithm | Security Standard |
| :---: | :--- | :--- |
| `$1$` | MD5 | Deprecated / Weak |
| `$2a$`, `$2y$` | Blowfish (bcrypt) | Strong |
| `$5$` | SHA-256 | Standard / Secure |
| `$6$` | SHA-512 | Standard High Security (Default on Linux) |
| `$y$` | yescrypt | Next-Gen Default on newer Debian/Ubuntu/Fedora |
