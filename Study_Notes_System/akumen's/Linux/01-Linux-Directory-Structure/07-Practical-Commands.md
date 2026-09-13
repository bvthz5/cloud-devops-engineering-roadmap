# 07 - Practical Commands for Inspecting Filesystems & Storage

Here is the essential toolbox of commands for exploring directories, inspecting files, analyzing storage consumption, and managing filesystems.

---

## 1. Directory Tree & Metadata Inspection

### `tree` — Visualize Hierarchies
```bash
# View directory tree 2 levels deep:
$ tree -L 2 /etc

# Show only directories, 3 levels deep, with human-readable sizes:
$ tree -d -L 3 -h /var

# Include file permissions and owners:
$ tree -p -u -g -L 2 /opt
```

### `ls` — Listing Essentials
```bash
# Detailed list with human-readable sizes, showing hidden files:
$ ls -lah

# Sort by file size (largest files first):
$ ls -laSh /var/log

# Sort by modification time (most recent first):
$ ls -lat /tmp

# Display inode numbers alongside files:
$ ls -lai /etc
```

### `stat` — Detailed Inode & Timestamp Inspection
The `stat` command exposes the exact contents of a file's inode:
```bash
$ stat /etc/hosts
  File: /etc/hosts
  Size: 384             Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d      Inode: 131074      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2026-09-13 10:15:22.124567890 +0000
Modify: 2026-09-12 18:30:00.000000000 +0000
Change: 2026-09-12 18:30:00.000000000 +0000
 Birth: 2026-09-01 08:00:00.000000000 +0000
```

### `file` — Determining File Type via Magic Bytes
Linux does not rely on file extensions like `.png` or `.txt`. The `file` command reads header magic numbers to determine true format:
```bash
$ file /bin/bash
/bin/bash: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked

$ file mystery_file
mystery_file: gzip compressed data, from Unix, original size modulo 2^32 10485760
```

---

## 2. Storage & Space Analysis Commands

### `df` — Disk Free (Filesystem Level)
```bash
# Human-readable view of all mounted filesystems:
$ df -h

# Include filesystem types (ext4, xfs, tmpfs):
$ df -hT

# Check INODE usage instead of disk space (critical for troubleshooting!):
$ df -i
```

### `du` — Disk Usage (Directory & File Level)
```bash
# Show summary size of current directory:
$ du -sh .

# Find the top 10 largest folders inside /var:
$ sudo du -h --max-depth=1 /var | sort -hr | head -n 10

# Scan total size of specific subfolders:
$ du -ch /var/log/*.log
```

---

## 3. High-Power File Searching with `find`

The `find` utility traverses the directory tree and evaluates criteria in real time.

```bash
# 1. Find all files larger than 500 Megabytes:
$ sudo find / -type f -size +500M -exec ls -lh {} + 2>/dev/null

# 2. Find all log files modified in the last 24 hours:
$ sudo find /var/log -name "*.log" -mtime -1

# 3. Find files modified more than 30 days ago and delete them:
$ sudo find /tmp -type f -mtime +30 -delete

# 4. Find all world-writable files (security audit):
$ sudo find / -perm -0002 -type f 2>/dev/null

# 5. Find files belonging to user "ubuntu" and change ownership:
$ sudo find /data -user ubuntu -exec chown deploy:deploy {} +

# 6. Find all broken/dangling symbolic links:
$ find / -xtype l 2>/dev/null
```

---

## 4. Binary & Command Path Resolvers

| Command | Purpose | Example | Output |
|---|---|---|---|
| **`which`** | Shows location of the executable in `$PATH` | `which python3` | `/usr/bin/python3` |
| **`whereis`** | Locates binary, source code, and man pages | `whereis nginx` | `/usr/sbin/nginx /etc/nginx /usr/share/man/...` |
| **`type`** | Reveals if a command is a shell built-in, alias, or disk binary | `type cd` | `cd is a shell builtin` |
| **`realpath`** | Resolves all symlinks to return canonical absolute path | `realpath /bin` | `/usr/bin` |
