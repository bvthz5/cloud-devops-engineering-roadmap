# SOURCE: Linux Folder Structure (FHS)

This document outlines the origin, foundational inputs, and structural expansions applied to create the **06-Linux-Folder-Structure** topic within the **Akumen Study System**.

---

## 📌 Primary Inputs & Scope

The foundation of this topic is built upon the official **Filesystem Hierarchy Standard (FHS)** and core Linux directory locations:

1. **Standard Directories Covered:**
   - `/` (Root directory)
   - `/boot` (Kernel, initrd, GRUB bootloader)
   - `/usr` (Unix System Resources: `/usr/bin`, `/usr/local`, `/usr/share`)
   - `/etc` (System configuration, `/etc/fstab`, systemd)
   - `/var` (Variable data: `/var/log`, `/var/lib`, `/var/spool`, `/var/www`)
   - `/home` & `/root` (User home directories vs superuser home directory)
   - `/opt` (Optional third-party software) & `/srv` (Service data)
   - `/tmp` & `/run` (Volatile temporary storage and runtime PID/socket files)
   - Virtual pseudo-filesystems: `/proc`, `/sys`, `/dev`
   - Mount points: `/mnt`, `/media`, `/data`
   - Usr-Merge symlinks: `/bin -> /usr/bin`, `/sbin -> /usr/sbin`, `/lib -> /usr/lib`

---

## 🛠️ Educational Expansions & DevOps Extensions

To transform standard directory definitions into a production-grade DevOps study guide, the original prompt material was systematically expanded to include:

- **Root vs Root User Home Distinction:** Explicit clarification between `/` (Filesystem Root) and `/root` (Superuser Home Directory).
- **Usr-Merge Architecture:** Historical context and modern migration rationale for symlinked `/bin`, `/sbin`, and `/lib`.
- **FHS 2x2 Mental Model Matrix:** Categorizing directories along *Static vs Variable* and *Shareable vs Unshareable* axes.
- **Production Partitioning Strategies:** Best practices for isolating `/var/log`, `/boot`, and `/tmp` in enterprise deployments.
- **Virtual Pseudo-Filesystem Mechanics:** In-depth breakdown of `procfs`, `sysfs`, and `devtmpfs`.
- **Assessment Suite:** Technical interview questions, hands-on terminal exercises, multiple-choice questions, and quick revision cheat sheets.

---

## 📂 Topic File Index

- [`README.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/README.md)
- [`01-Filesystem-Root.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/01-Filesystem-Root.md)
- [`02-boot.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/02-boot.md)
- [`03-usr.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/03-usr.md)
- [`04-etc.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/04-etc.md)
- [`05-var.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/05-var.md)
- [`06-home-and-root.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/06-home-and-root.md)
- [`07-opt-and-srv.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/07-opt-and-srv.md)
- [`08-tmp-and-run.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/08-tmp-and-run.md)
- [`09-proc-sys-and-dev.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/09-proc-sys-and-dev.md)
- [`10-mnt-media-and-data.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/10-mnt-media-and-data.md)
- [`11-Symbolic-Links-bin-sbin-lib.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/11-Symbolic-Links-bin-sbin-lib.md)
- [`12-FHS-Mental-Model.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/12-FHS-Mental-Model.md)
- [`13-Useful-Commands.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/13-Useful-Commands.md)
- [`14-Real-World-Scenarios.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/14-Real-World-Scenarios.md)
- [`15-Troubleshooting.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/15-Troubleshooting.md)
- [`16-Interview-QA.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/16-Interview-QA.md)
- [`17-Hands-On-Practice.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/17-Hands-On-Practice.md)
- [`18-MCQ.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/18-MCQ.md)
- [`19-Quick-Revision.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/19-Quick-Revision.md)
- [`20-Related-Topics.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/20-Related-Topics.md)
