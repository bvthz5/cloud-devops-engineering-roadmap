# 14 - Links & File Metadata (`ln`, `stat`, `touch`)

Understanding how links map to inodes is essential for configuring system binaries, managing shared libraries, and troubleshooting disk usage.

---

## 1. Hard Links vs. Symbolic (Soft) Links

Linux provides two distinct mechanisms to reference files:

```
HARD LINK MECHANISM:
Directory Entry A ("original.txt") ──┐
                                     ├───► Inode #1048577 ───► [ Physical Disk Data ]
Directory Entry B ("hardlink.txt")  ──┘

SYMBOLIC LINK MECHANISM:
Directory Entry A ("original.txt") ──────► Inode #1048577 ───► [ Physical Disk Data ]
                                                 ▲
Directory Entry C ("symlink.txt") ──► Inode #2097153 (Contains Path String: "original.txt")
```

### Comparative Breakdown:

| Characteristic | Hard Link (`ln target link`) | Symbolic Link (`ln -s target link`) |
|---|---|---|
| **Inode Representation** | Shares the **exact same inode number** as the target file. | Has its own **unique inode number** with file type `l`. |
| **Cross-Filesystem** | **No.** Cannot cross partition boundaries. | **Yes.** Can point to files on different disks or NFS mounts. |
| **Directory Linking** | **No.** Forbidden to prevent infinite recursive loops. | **Yes.** Can link to directories seamlessly. |
| **If Target is Deleted**| **Data survives!** Data remains accessible via hard link. | **Broken link!** Displays "No such file or directory" error. |
| **Disk Overhead** | Zero extra disk space used (just a directory entry). | Small file containing the target path string (a few bytes). |

---

## 2. Practical Link Commands

```bash
# 1. Create a Hard Link:
$ ln /var/log/app.log /var/log/app_mirror.log

# Verify hard link count increases:
$ ls -l /var/log/app.log /var/log/app_mirror.log
-rw-r--r-- 2 root root 4096 Sep 13 10:00 /var/log/app.log
-rw-r--r-- 2 root root 4096 Sep 13 10:00 /var/log/app_mirror.log
# (Notice the link count '2' and matching inode numbers!)

# 2. Create a Symbolic (Soft) Link:
$ ln -s /usr/bin/python3 /usr/local/bin/python

# Verify symlink output:
$ ls -l /usr/local/bin/python
lrwxrwxrwx 1 root root 16 Sep 13 10:05 /usr/local/bin/python -> /usr/bin/python3

# 3. Find all broken dangling symlinks on the system:
$ find / -xtype l 2>/dev/null
```

---

## 3. `stat` — Deep Inode Metadata Inspection

While `ls -l` provides a summary, `stat` prints the complete low-level inode fields:

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

---

## 4. `touch` — Creating Files & Manipulating Timestamps

```bash
# 1. Create an empty file (or update timestamps if file exists):
$ touch newfile.txt

# 2. Update ONLY Access time (atime):
$ touch -a newfile.txt

# 3. Update ONLY Modification time (mtime):
$ touch -m newfile.txt

# 4. Set timestamp to a specific historic or future date (YYYYMMDDhhmm.ss):
$ touch -t 202501011200.00 old_archive.tar.gz
```
