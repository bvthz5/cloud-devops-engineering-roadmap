# 13 - Useful Inspection Commands for Filesystem Structure

A essential collection of CLI utilities for inspecting, navigating, and measuring the Linux directory hierarchy.

---

## 🛠️ Command Reference Table

| Command | Purpose | Useful Examples |
| :--- | :--- | :--- |
| `tree` | Visualizes directory trees recursively. | `tree -L 2 /` (Limit depth to 2 levels) |
| `df` | Reports disk space usage of mounted filesystems. | `df -hT` (Human readable + filesystem types) |
| `du` | Measures disk space consumed by files/directories. | `du -sh /var/* \| sort -hr` (Top 10 largest folders) |
| `lsblk` | Lists block devices, partitions, and mount points. | `lsblk -f` (Shows UUIDs and filesystem formats) |
| `findmnt` | Displays current filesystem mount tree hierarchy. | `findmnt --real` (Excludes virtual pseudo-filesystems) |
| `stat` | Displays detailed inode and file metadata. | `stat /etc/fstab` |
| `file` | Identifies file type independently of extension. | `file /bin/ls` (Shows 64-bit ELF binary details) |

---

## 💻 Code Recipes & Command Workflows

```bash
# 1. View top-level directory structure with tree
tree -d -L 1 /

# 2. Find the top 5 largest directories consuming space in /var
sudo du -h --max-depth=1 /var | sort -hr | head -n 5

# 3. Check mount details for root / and /boot
findmnt /
findmnt /boot

# 4. Check if /var/log is a separate partition
df -h /var/log
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - FHS Mental Model](./12-FHS-Mental-Model.md) | [README](./README.md) | [14 - Real World Scenarios](./14-Real-World-Scenarios.md) |
