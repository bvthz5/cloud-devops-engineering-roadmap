# 13 - Permissions, Ownership & Special Bits (`chmod`, `chown`, `umask`)

Linux enforces a multi-user POSIX security model. Every file and directory is bound to an owner user, a group, and a 9-bit permission mask.

---

## 1. The 9-Bit Permission Mask & User Categories

When you run `ls -l /etc/passwd`:
```text
- rw- r-- r-- 1 root root 2841 Sep 13 10:00 /etc/passwd
│  │   │   │
│  │   │   └── Others (o): Read-only (r--)
│  │   └────── Group  (g): Read-only (r--)
│  └────────── Owner  (u): Read & Write (rw-)
└───────────── File Type: Regular File (-)
```

### The Octal Permission Value Table:

| Binary | Octal | Symbolic | Permission Granted |
|:---:|:---:|:---:|---|
| `000` | **`0`** | `---` | No permissions allowed. |
| `100` | **`4`** | `r--` | Read permission only. |
| `010` | **`2`** | `-w-` | Write permission only. |
| `001` | **`1`** | `--x` | Execute permission only. |
| `110` | **`6`** | `rw-` | Read & Write (4 + 2). Standard for data files. |
| `101` | **`5`** | `r-x` | Read & Execute (4 + 1). Standard for executable dirs/scripts. |
| `111` | **`7`** | `rwx` | Read, Write & Execute (4 + 2 + 1). Full permissions. |

---

## 2. Directory Permissions Nuances (Crucial!)

Permissions on a directory behave differently than permissions on a regular file:

```
┌─────────────────────────────────────────────────────────────┐
│ DIRECTORY PERMISSION BEHAVIOR:                              │
├─────────────────┬───────────────────────────────────────────┤
│ Read (`r`)      │ Grants permission to list file names inside│
│                 │ the directory using `ls`.                 │
├─────────────────┼───────────────────────────────────────────┤
│ Write (`w`)     │ Grants permission to CREATE, DELETE, and  │
│                 │ RENAME files inside the directory!        │
├─────────────────┼───────────────────────────────────────────┤
│ Execute (`x`)   │ Grants permission to ENTER/TRAVERSE the   │
│                 │ directory using `cd` and access metadata. │
└─────────────────┴───────────────────────────────────────────┘
```
> [!WARNING]
> If a user has Write (`w`) permission on a directory, **they can delete files inside that directory**, even if they do not own the files or have write permissions on the individual files!

---

## 3. `chmod` & `chown` Usage

### Symbolic vs. Octal `chmod`:
```bash
# Octal Mode Examples:
$ chmod 755 deploy.sh       # Owner: rwx, Group: r-x, Others: r-x
$ chmod 644 config.yml      # Owner: rw-, Group: r--, Others: r--
$ chmod 600 id_rsa          # Owner: rw-, Group: ---, Others: ---

# Symbolic Mode Examples:
$ chmod u+x script.sh       # Add execute permission to User/Owner
$ chmod g-w file.txt        # Remove write permission from Group
$ chmod o=r file.txt        # Set Others permission strictly to Read
$ chmod -R 755 /var/www/    # Recursively apply permissions to folder
```

### `chown` & `chgrp` (Ownership Management):
```bash
# Change file owner:
$ sudo chown ubuntu app.py

# Change file owner AND group simultaneously:
$ sudo chown www-data:www-data /var/www/html/index.html

# Recursively change ownership for an entire application directory:
$ sudo chown -R deploy:deploy /opt/my_app/
```

---

## 4. Default Creation Mask: `umask`

When a process creates a new file or directory, the kernel calculates initial permissions by subtracting the **`umask`** value from base defaults:
- **Base File Default:** `666` (`rw-rw-rw-`)
- **Base Directory Default:** `777` (`rwxrwxrwx`)

```bash
# Default system umask is typically 022:
$ umask
0022

# File Creation Calculation: 666 - 022 = 644 (rw-r--r--)
# Directory Creation Calculation: 777 - 022 = 755 (rwxr-xr-x)
```

---

## 5. Special Permissions: SUID, SGID, and Sticky Bit

| Special Bit | Octal | On Executable File | On Directory | Example |
|---|:---:|---|---|---|
| **SUID** (Set User ID) | `4000` | Runs binary with privileges of **file owner** (e.g. root). | N/A | `/usr/bin/passwd` (`-rwsr-xr-x`) |
| **SGID** (Set Group ID) | `2000` | Runs binary with privileges of **file group**. | **Files created inside automatically inherit directory's group!** | Shared team directory (`chmod 2775 /data`) |
| **Sticky Bit** | `1000` | N/A | **Users can ONLY delete files they own** inside the directory! | `/tmp` (`drwxrwxrwt`) |

```bash
# Setting Sticky Bit on shared tmp directory:
$ sudo chmod 1777 /shared_tmp/
# OR:
$ sudo chmod +t /shared_tmp/
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Paths Wildcards and Expansion](./12-Paths-Wildcards-and-Expansion.md) | [README](./README.md) | [14 - Links and Metadata](./14-Links-and-Metadata.md) |
