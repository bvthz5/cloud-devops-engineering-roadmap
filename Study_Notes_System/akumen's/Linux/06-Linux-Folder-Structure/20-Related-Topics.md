# 20 - Related Topics & Next Steps: Linux Folder Structure

Understanding the Filesystem Hierarchy Standard connects directly into advanced Linux storage management, security hardening, and Cloud/DevOps infrastructure practices.

---

## 🔗 Related Akumen Study Topics

| Topic | Description | Link / Location |
| :--- | :--- | :--- |
| **File Management in Linux** | Commands (`ls`, `cd`, `cp`, `mv`, `rm`, `find`, `grep`) used to interact with directory paths. | `05-File-Management-in-Linux` |
| **Linux Kernel** | Explains virtual filesystems (`procfs`, `sysfs`, `VFS`) built into kernel space. | `04-Kernel` |
| **Linux Architecture** | System call mechanics (`open()`, `read()`, `write()`) and hardware abstraction layers. | `02-Linux-Architecture` |
| **Linux Storage & LVM** | Logical Volume Manager (LVM), partitioning (`fdisk`, `parted`), mounting (`/etc/fstab`), and XFS/Ext4 filesystems. | *Upcoming Topic* |
| **Linux Security & Permissions** | File modes (`chmod`), ownership (`chown`), Access Control Lists (ACLs), SELinux / AppArmor contexts. | *Upcoming Topic* |

---

## 🚀 Recommended Next Steps in Learning Path

1. **Practice Partitioning & LVM:**
   Learn how to add secondary disks, create LVM physical volumes (`pvcreate`), volume groups (`vgcreate`), logical volumes (`lvcreate`), and format them with Ext4/XFS filesystems.
2. **Master Mount Configurations (`/etc/fstab`):**
   Practice adding persistent auto-mount entries using disk UUIDs (`blkid`).
3. **Container Storage Drivers & Bind Mounts:**
   Explore how Docker and Kubernetes leverage Linux folder structures for container volume mounts (`-v /host/dir:/container/dir`).

---

## ⬅️ Navigation
- Previous: [19 - Quick Revision Cheat Sheet](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/19-Quick-Revision.md)
- Topic Index: [README.md](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/README.md)
