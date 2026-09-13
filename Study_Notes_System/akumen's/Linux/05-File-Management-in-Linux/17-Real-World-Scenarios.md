# 17 - Real-World Production Scenarios & Automation

Mastering file management enables SREs to build zero-downtime deployment pipelines and automate log management.

---

## 1. Zero-Downtime Application Deployment via Symlink Swapping

### The Goal:
Deploy version `v2.0` of a web application to `/var/www/html` without shutting down the web server or serving HTTP 404 errors during the file copy.

### The Solution:
Use an **atomic symbolic link swap** with `ln -sfn`:

```
/var/www/
├── releases/
│   ├── v1.0/
│   └── v2.0/   <-- New deployment copied here
└── current ───► Symbolic link pointing to /var/www/releases/v2.0
```

```bash
# 1. Extract new code into a new version directory:
$ sudo tar -xzvf release-v2.0.tar.gz -C /var/www/releases/v2.0/

# 2. Atomically swap the symlink to point to v2.0:
$ sudo ln -sfn /var/www/releases/v2.0 /var/www/current

# 3. If v2.0 has a bug, instantly rollback to v1.0 in 1 millisecond!
$ sudo ln -sfn /var/www/releases/v1.0 /var/www/current
```
- **`-n` (no-dereference):** Treats target symlink as a normal file so it is replaced rather than nesting inside.
- **`-f` (force):** Forces atomic overwrite of the link.

---

## 2. Safe Zeroing of Live Log Files

### The Problem:
A disk reaches 98% full due to a runaway 50 GB log file `/var/log/app.log`.
If you run `rm /var/log/app.log`, the application process still holds the open file descriptor, so disk space is **not freed** and the app crashes when it fails to find its log file.

### The Production Remedy:
Truncate the file in place without deleting the inode or file descriptor:

```bash
# Option 1: Truncate utility:
$ sudo truncate -s 0 /var/log/app.log

# Option 2: Shell colon redirection operator:
$ sudo bash -c ': > /var/log/app.log'
```
Disk space is reclaimed instantly, and the running application continues logging without interruption!

---

## 3. Mass Security Permission Hardening

### The Problem:
A developer uploads an application folder where every file and folder was assigned `chmod 777` (world-writable security violation).

### The Production Remedy:
Fix permissions in bulk using `find` with `-type d` and `-type f`:

```bash
# 1. Set all DIRECTORIES to 755 (rwxr-xr-x - owner full, others read/traverse):
$ sudo find /var/www/app -type d -exec chmod 755 {} +

# 2. Set all REGULAR FILES to 644 (rw-r--r-- - owner read/write, others read):
$ sudo find /var/www/app -type f -exec chmod 644 {} +

# 3. Grant execute permission strictly to shell scripts:
$ sudo find /var/www/app -type f -name "*.sh" -exec chmod +x {} +
```
