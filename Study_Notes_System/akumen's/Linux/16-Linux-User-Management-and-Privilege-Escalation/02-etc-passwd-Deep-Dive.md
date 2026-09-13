# 02 - /etc/passwd Deep Dive

`/etc/passwd` is one of the most important files on any Linux system. Despite its name, it does **not** store passwords (those are in `/etc/shadow`). It stores the fundamental identity mapping for every user account on the system.

---

## 🔍 Reading `/etc/passwd`

```bash
cat /etc/passwd
```

Each line represents one user account. Fields are separated by colons (`:`).

---

## 📐 The 7 Fields

```text
alice:x:1001:1001:Alice Smith,,,:/home/alice:/bin/bash
  1   2  3    4        5            6          7
```

| Field # | Name | Description |
| :---: | :--- | :--- |
| **1** | **Username** | The login name (e.g., `alice`). |
| **2** | **Password Placeholder** | Almost always `x`. This indicates the actual password hash is stored in `/etc/shadow`. If this field contained anything else (like `!` or an actual hash), it would be a legacy system or a security misconfiguration. |
| **3** | **UID** | The numeric User ID (e.g., `1001`). |
| **4** | **GID** | The numeric Group ID of the user's **primary group** (e.g., `1001`). |
| **5** | **GECOS / Comment** | A description field, typically the user's full name. Originally stood for "General Electric Comprehensive Operating Supervisor" (a historical relic). Subfields (separated by commas) can include Room Number, Work Phone, Home Phone. |
| **6** | **Home Directory** | The absolute path to the user's home directory (e.g., `/home/alice`). This is set as the `HOME` environment variable upon login. |
| **7** | **Login Shell** | The shell that starts when the user logs in (e.g., `/bin/bash`). For service accounts that should never log in interactively, this is set to `/sbin/nologin` or `/bin/false`. |

---

## 🔑 Important System Entries

```text
root:x:0:0:root:/root:/bin/bash
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

*   **`root`**: UID 0. The superuser. Home directory is `/root` (not `/home/root`).
*   **`nobody`**: A special account with minimal privileges. Used by some daemons as a safety measure.
*   **`www-data`**: The service account for the Apache/Nginx web server. Note the shell is `/usr/sbin/nologin` — this prevents anyone from using this account to get an interactive terminal.

---

## ⚠️ Security Notes

1.  **World-Readable:** `/etc/passwd` is readable by every user on the system (`-rw-r--r--`). This is intentional — programs like `ls` need to map UIDs to usernames.
2.  **No Passwords Here:** If you ever see an actual hash in Field 2 instead of `x`, the system is using an insecure legacy configuration. Passwords must be in the shadow file.
3.  **UID 0 Audit:** Any account with UID `0` in Field 3 has full root privileges. A common security audit is to grep for additional UID 0 accounts:
    ```bash
    awk -F: '$3 == 0 {print $1}' /etc/passwd
    # Should ONLY return 'root'.
    ```
