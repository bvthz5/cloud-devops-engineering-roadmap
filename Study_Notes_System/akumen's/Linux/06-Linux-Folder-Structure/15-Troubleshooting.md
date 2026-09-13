# 15 - Troubleshooting Filesystem Structure & Mount Issues

A guide to diagnosing and resolving common Linux folder structure failures, disk full panics, broken mount points, and orphaned symlinks.

---

## 🛠️ Symptom & Remediation Guide

### 1. `df -h` shows 100% full root (`/`), but `du -sh /*` shows low disk space usage

#### Root Cause: Deleted files held open by running processes
When a log file is deleted with `rm` while a background process (like Nginx or Java) still has an open file descriptor pointing to it, the storage blocks are not freed by the kernel until the process releases the handle.

#### Resolution Workflow:
```bash
# Find deleted files still held open by processes
sudo lsof +L1

# Restart the process holding the deleted file to release disk blocks
sudo systemctl restart nginx
```

---

### 2. `/boot` is 100% full, preventing package manager updates (`apt` / `yum`)

#### Root Cause: Old Linux kernel versions accumulate in `/boot` over time without cleanup.

#### Resolution Workflow:
```bash
# Check space on /boot
df -h /boot

# Remove old unused kernels on Ubuntu/Debian safely
sudo apt-get autoremove --purge
```

---

### 3. "No space left on device" error when `df -h` shows disk space is available

#### Root Cause: Inode exhaustion. Hundreds of thousands of tiny files consumed all available filesystem inodes.

#### Resolution Workflow:
```bash
# Check inode utilization instead of disk space
df -i

# Find directories containing the largest number of files
find /var/spool -xdev -printf '%h\n' | sort | uniq -c | sort -nr | head -n 10
```

---

### 4. Broken or Dangling Symlink in `/bin` or `/usr/bin`

#### Root Cause: Target binary moved or deleted after creating a symbolic link.

#### Resolution Workflow:
```bash
# Find all broken symbolic links starting from /
find / -xtype l -ls 2>/dev/null
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Real World Scenarios](./14-Real-World-Scenarios.md) | [README](./README.md) | [16 - Interview QA](./16-Interview-QA.md) |
