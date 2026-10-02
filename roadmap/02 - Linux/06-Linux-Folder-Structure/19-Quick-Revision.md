# 19 - Quick Revision Cheat Sheet: Linux Folder Structure

A 5-minute high-density reference sheet for Linux directory paths and FHS definitions.

---

## 🚀 Directory Quick-Reference Matrix

| Path | Primary Function | Disk Usage / Persistence |
| :--- | :--- | :--- |
| **`/`** | Absolute root of the single unified directory tree. | Primary disk partition. |
| **`/boot`** | Bootloader, GRUB files, kernel images (`vmlinuz`, `initrd`). | Physical disk (often separate partition). |
| **`/etc`** | System-wide plain-text configuration files. | Physical disk (`/`). |
| **`/var`** | Variable data: logs (`/var/log`), DB files (`/var/lib`). | Physical disk (often separate partition). |
| **`/usr`** | Unix System Resources: binaries (`/usr/bin`), shared libraries (`/usr/lib`). | Physical disk. |
| **`/home`** | Home directories for regular system users (`/home/username`). | Physical disk (often separate partition). |
| **`/root`** | Home directory for the administrative `root` user. | Physical disk (`/`). |
| **`/opt`** | Self-contained 3rd-party software packages (GitLab, Chrome). | Physical disk. |
| **`/srv`** | Site-specific data served to clients (Web, FTP, Git). | Physical disk. |
| **`/tmp`** | Temporary scratch space (`1777` Sticky Bit permissions). | RAM (`tmpfs`) or cleared on reboot. |
| **`/run`** | Runtime state data since last boot (PID files, sockets, locks). | RAM (`tmpfs`) - cleared on reboot. |
| **`/proc`** | Kernel & process virtual state pseudo-filesystem. | 0 bytes on disk (RAM virtual). |
| **`/sys`** | Hardware device drivers & bus topology pseudo-filesystem. | 0 bytes on disk (RAM virtual). |
| **`/dev`** | Device node files (block devices `/dev/sda`, character `/dev/tty`). | Virtual (`devtmpfs`). |
| **`/mnt`** | Temporary mount point location for SysAdmins. | Disk mount target. |
| **`/media`** | Automount location for removable media (USB drives, CDs). | Disk mount target. |
| **`/bin`** | Symlink to `/usr/bin` under modern Usr-Merge architecture. | Symlink. |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [18 - MCQ](./18-MCQ.md) | [README](./README.md) | [20 - Related Topics](./20-Related-Topics.md) |
