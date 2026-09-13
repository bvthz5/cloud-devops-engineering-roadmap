# 03 - /etc/shadow Deep Dive

`/etc/shadow` is the most security-sensitive identity file on a Linux system. It stores the actual password hashes and password aging policies. Unlike `/etc/passwd`, it is readable **only by root** (`-rw-r-----`).

---

## 🔍 Reading `/etc/shadow`

```bash
sudo cat /etc/shadow
```

Each line corresponds to one user account, matching the order in `/etc/passwd`.

---

## 📐 The 9 Fields

```text
alice:$6$xyz$longHashString:19500:0:99999:7:30:19900:
  1          2               3   4   5   6  7   8   9
```

| Field # | Name | Description |
| :---: | :--- | :--- |
| **1** | **Username** | Matches Field 1 of `/etc/passwd`. |
| **2** | **Password Hash** | The encrypted password. This is the most critical field. See hash formats below. Special values: `!` or `*` means the account is locked / no password login allowed. `!!` means the password has never been set. |
| **3** | **Last Changed** | The date the password was last changed, expressed as the number of days since the Unix Epoch (January 1, 1970). |
| **4** | **Minimum Age** | The minimum number of days between password changes. `0` means the user can change it anytime. |
| **5** | **Maximum Age** | The maximum number of days a password is valid. After this, the user is forced to change it. `99999` effectively means "never expires". |
| **6** | **Warning Period** | The number of days before password expiration that the user is warned (e.g., "Your password expires in 7 days"). |
| **7** | **Inactivity Period** | The number of days after password expiration before the account is completely disabled. |
| **8** | **Expiration Date** | The absolute date the account expires (days since Epoch), regardless of password status. After this date, the user cannot log in at all. |
| **9** | **Reserved** | Unused, reserved for future use. |

---

## 🔐 Password Hash Formats

The hash in Field 2 follows a structured format: `$id$salt$hash`

| ID | Algorithm | Security Level | Notes |
| :---: | :--- | :---: | :--- |
| `$1$` | MD5 | ❌ Weak | Easily crackable. Legacy systems only. |
| `$5$` | SHA-256 | ✅ Good | Modern and secure. |
| `$6$` | SHA-512 | ✅✅ Best | The current default on most modern Linux distributions. |
| `$y$` | yescrypt | ✅✅✅ Strongest | Newer memory-hard algorithm. Used by latest Debian/Ubuntu. |

**The Salt:** A random string injected into the hashing process. It ensures that two users with the same password get different hashes, defeating precomputed "rainbow table" attacks.

---

## 🔒 Account Locking Indicators

If the hash field starts with `!` or `!!`:

*   **`!`**: The account's password has been locked (using `passwd -l` or `usermod -L`). The `!` is prepended to the existing hash, disabling it.
*   **`!!`**: The password has never been set. The account exists but cannot be used for password-based login.
*   **`*`**: The account is a system/service account that was never intended for password-based login.

To unlock a locked account, use `passwd -u <username>` or `usermod -U <username>`.

---

## 👁️ Viewing Human-Readable Password Info

Use the `chage` command to display the shadow file's aging fields in a human-readable format:

```bash
sudo chage -l alice
```

*Output:*
```text
Last password change                                    : May 10, 2026
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```
