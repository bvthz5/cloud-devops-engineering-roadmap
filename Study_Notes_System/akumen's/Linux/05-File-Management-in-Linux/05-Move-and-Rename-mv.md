# 05 - Moving & Renaming Files: `mv`

The `mv` utility serves two roles in Linux: **renaming** files/directories and **relocating** them across paths or filesystems.

---

## 1. Renaming vs. Moving

```bash
# 1. Renaming a file (stays in the same folder):
$ mv server.conf server.conf.old

# 2. Moving a file to an existing destination directory:
$ mv deploy.sh /usr/local/bin/

# 3. Moving and renaming simultaneously:
$ mv /tmp/download.tar.gz /var/backups/archive-2026.tar.gz

# 4. Moving multiple files into a directory:
$ mv *.log /var/log/archive/
```

---

## 2. Core Flags & Safety Options

| Flag | Meaning | Behavior |
|:---:|---|---|
| **`-i`** | Interactive | Prompts before overwriting an existing destination file. |
| **`-f`** | Force | Silently overwrites destination files without confirmation prompts. |
| **`-n`** | No-Clobber | **Never overwrites** an existing destination file. |
| **`-u`** | Update | Moves only if the source is **newer** than the destination or if the destination file is missing. |
| **`-v`** | Verbose | Prints the source and destination for each moved file. |
| **`-b`** | Backup | Automatically creates a backup (`file~`) before overwriting a destination file. |

```bash
# Example of non-destructive backup moving:
$ mv -b nginx.conf /etc/nginx/
# If /etc/nginx/nginx.conf already existed, it is saved as nginx.conf~
```

---

## 3. Under the Hood: Same Filesystem vs. Cross-Filesystem Moves

Understanding how the Linux kernel processes `mv` explains why moving a 100 GB file can be instantaneous or take several minutes:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. Moving on the SAME Filesystem (Same Partition):          │
│ • ATOMIC OPERATION!                                         │
│ • The kernel merely updates the filename in the directory   │
│   entry and points it to the existing Inode.                │
│ • Zero disk blocks are copied. It takes 1 millisecond       │
│   whether the file is 10 bytes or 500 Gigabytes!            │
├─────────────────────────────────────────────────────────────┤
│ 2. Moving ACROSS Filesystems (e.g., /home to /mnt/backup):  │
│ • NON-ATOMIC OPERATION.                                     │
│ • The kernel must copy every data block across the bus to   │
│   a new inode on the destination partition, verify the write│
│   and then delete (unlink) the source file.                 │
└─────────────────────────────────────────────────────────────┘
```
