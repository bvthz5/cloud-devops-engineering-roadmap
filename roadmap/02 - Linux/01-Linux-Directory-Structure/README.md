# Linux Directory Structure & Filesystem Hierarchy Standard (FHS)

Welcome to the comprehensive study module on the **Linux Filesystem Hierarchy Standard (FHS)**. This module deconstructs how Linux organizes data, binaries, system configurations, virtual filesystems, and storage mount points.

---

## 🗺️ Learning Roadmap & Mind Map

```
Linux Unified Virtual Directory Tree
                │
                └── / (Root)
                    ├── /bin  ───> [symlink in modern distros to /usr/bin]
                    ├── /sbin ───> [symlink in modern distros to /usr/sbin]
                    ├── /lib  ───> [symlink in modern distros to /usr/lib]
                    │
                    ├── Core OS & Boot:
                    │   ├── /boot (GRUB, kernel vmlinuz, initramfs)
                    │   └── /etc  (Host-specific configs, systemd, networking)
                    │
                    ├── Virtual & Pseudo Filesystems (Kernel / Hardware):
                    │   ├── /dev  (Device nodes managed by udev: sda, tty, urandom)
                    │   ├── /proc (Kernel parameters, process IDs, hardware info)
                    │   ├── /sys  (Kernel subsystem, device drivers, power mgmt)
                    │   └── /run  (Volatile runtime data since boot, tmpfs)
                    │
                    ├── User Spaces & Packages:
                    │   ├── /home (Non-root user spaces: /home/student)
                    │   ├── /root (Superuser root home directory)
                    │   ├── /usr  (User programs, libraries, shared assets)
                    │   │   └── /usr/local (Locally compiled software)
                    │   └── /opt  (Third-party self-contained add-on packages)
                    │
                    ├── Variable & Ephemeral Data:
                    │   ├── /var  (Logs, spool, database state, containers)
                    │   │   ├── /var/log
                    │   │   ├── /var/lib (e.g., /var/lib/docker)
                    │   │   └── /var/spool
                    │   ├── /tmp  (Temporary files, often cleaned on reboot)
                    │   └── /srv  (Site-specific service data)
                    │
                    └── Mount Points:
                        ├── /mnt   (Temporary manual mounts for sysadmins)
                        └── /media (Automount point for removable media like USB/CD)
```

---

## 🎯 Module Objectives

By the end of this module, you will:
1. **Understand the Single Tree Concept:** Learn how Linux unifies diverse physical storage, network shares, and virtual memory into a single inverted tree rooted at `/`.
2. **Master the FHS Specification:** Differentiate between static vs. variable files, sharable vs. unsharable data.
3. **Eliminate Common Confusions:** Clarify `/` vs `/root`, `/bin` vs `/usr/bin` (Merged `/usr`), `/tmp` vs `/var/tmp`, `/opt` vs `/usr/local`, and `/mnt` vs `/media`.
4. **Demystify Virtual Filesystems:** Understand why files in `/proc`, `/sys`, and `/dev` take 0 bytes on disk yet provide vital live kernel metrics.
5. **Inspect & Manage Storage:** Use `df`, `du`, `find`, `lsblk`, `mount`, and analyze `/etc/fstab`.
6. **DevOps & Production Readiness:** Triage disk-full emergencies, inode exhaustion, runaway logs in `/var/log`, and container volume bindings.
7. **Excel in Technical Interviews:** Tackle real-world scenario questions asked in DevOps and SRE interviews.

---

## 📋 Module Index

| File | Topic Covered | Purpose |
|---|---|---|
| [01-Filesystem-Basics.md](./01-Filesystem-Basics.md) | Inverted Tree, Everything is a File, Inodes | Foundational concepts & mental model |
| [02-Directory-Structure.md](./02-Directory-Structure.md) | The Standard 18 Directories | Master breakdown of every top-level dir |
| [03-Paths-and-Navigation.md](./03-Paths-and-Navigation.md) | Absolute vs. Relative Paths, `.` and `..`, `~` | Fast and accurate navigation skills |
| [04-Important-Directories-Deep-Dive.md](./04-Important-Directories-Deep-Dive.md) | `/etc`, `/var`, `/proc`, `/sys`, `/dev` | Deep dive into system-critical internals |
| [05-FHS-and-Modern-Linux.md](./05-FHS-and-Modern-Linux.md) | FHS Matrix, Merged `/usr`, systemd `/run` | Modern Linux standards & architectural shifts |
| [06-Mounts-and-Filesystems.md](./06-Mounts-and-Filesystems.md) | Mounting, `/etc/fstab`, UUIDs, VFS | How disks attach to directory nodes |
| [07-Practical-Commands.md](./07-Practical-Commands.md) | `ls`, `tree`, `find`, `du`, `df`, `stat`, `file` | Day-to-day command recipes & syntax |
| [08-Real-World-Scenarios.md](./08-Real-World-Scenarios.md) | Production setups, container mounts, EBS volumes | Real enterprise architectures & best practices |
| [09-Troubleshooting.md](./09-Troubleshooting.md) | Disk 100% full, deleted open files, inode exhaustion | Practical SRE/DevOps incident response guides |
| [10-Interview-QA.md](./10-Interview-QA.md) | Scenario & Technical Interview Questions | Junior to Senior level QA |
| [11-Hands-On-Practice.md](./11-Hands-On-Practice.md) | Lab exercises, step-by-step challenges | Hands-on terminal muscle memory |
| [12-MCQ.md](./12-MCQ.md) | Multiple Choice Questions with Explanations | Self-assessment knowledge checks |
| [13-Quick-Revision.md](./13-Quick-Revision.md) | 5-Minute Cheat Sheet, Comparison Tables | Rapid pre-interview / exam review |
| [14-Related-Topics.md](./14-Related-Topics.md) | Permissions, LVM, Inodes, Systemd, Containers | Next learning path connections |

---

## 💡 Prerequisites
- Basic familiarity with terminal/command line interface.
- Basic understanding of how computer operating systems store files.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| — | You are here | [01 - Filesystem Basics](./01-Filesystem-Basics.md) |
