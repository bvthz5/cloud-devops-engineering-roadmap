# 11 - Usr-Merge Symlinks: `/bin`, `/sbin`, `/lib`

In modern Linux distributions (such as Ubuntu 20.04+, Debian 12+, RHEL 9+, Arch Linux, Fedora), top-level root directories `/bin`, `/sbin`, and `/lib` are no longer independent physical folders. They are **Symbolic Links** pointing to their counterparts inside `/usr`.

---

## 🔗 The Usr-Merge Architecture

```text
Modern Usr-Merge Layout:
  /bin  ──(symlink)──> /usr/bin
  /sbin ──(symlink)──> /usr/sbin
  /lib  ──(symlink)──> /usr/lib
  /lib64 ──(symlink)──> /usr/lib64
```

---

## 📜 Historical Background: Why were they originally separated?

In the early days of Unix (1970s):
- The original PDP-11 hard disk drives were extremely small (e.g., 1.5 MB total capacity).
- When the primary disk filled up, developers attached a second disk drive and mounted it to `/usr`.
- Essential boot binaries needed to repair a system in single-user mode were kept on the root disk inside `/bin` and `/sbin`. Non-essential binaries were installed on the second disk inside `/usr/bin`.

Over time, this separation led to duplicate libraries, broken dependencies when `/usr` failed to mount, and complex packaging scripts.

---

## 🚀 Benefits of Modern Usr-Merge

1. **Simplified System Backups & Snapshots:** All system binaries and shared libraries reside in a single tree under `/usr`, allowing easy read-only OS snapshots or network booting.
2. **Eliminates Duplicate Binaries:** Solves confusion over whether a utility belongs in `/bin` or `/usr/bin`.
3. **Backward Compatibility:** Legacy scripts calling `#!/bin/bash` or `/bin/ls` continue to execute flawlessly because `/bin` redirects seamlessly to `/usr/bin`.

---

## 🔍 Verifying Usr-Merge on Your System

```bash
# Check if /bin, /sbin, and /lib are symbolic links
ls -ld /bin /sbin /lib /lib64

# Example output on modern distro:
# lrwxrwxrwx 1 root root 7 Jan 10 12:00 /bin -> usr/bin
# lrwxrwxrwx 1 root root 8 Jan 10 12:00 /sbin -> usr/sbin
# lrwxrwxrwx 1 root root 7 Jan 10 12:00 /lib -> usr/lib
```

---

## ⬅️ Navigation
- Previous: [10 - Storage Mount Points](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/10-mnt-media-and-data.md)
- Next: [12 - FHS Mental Model & Categorization](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/12-FHS-Mental-Model.md)
