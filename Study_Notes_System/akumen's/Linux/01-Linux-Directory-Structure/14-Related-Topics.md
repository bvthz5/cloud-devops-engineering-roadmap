# 14 - Related Topics & Next Steps in the DevOps Roadmap

Mastering the Linux Directory Structure unlocks the core mental model required for advanced systems engineering, cloud administration, and container orchestration.

---

## 🗺️ Next Topics in the Cloud & DevOps Roadmap

```
               ┌────────────────────────────────────────────────────────┐
               │ 01-Linux-Directory-Structure (Current Completed Module)│
               └───────────────────────────┬────────────────────────────┘
                                           │
             ┌─────────────────────────────┼─────────────────────────────┐
             ▼                             ▼                             ▼
┌─────────────────────────┐   ┌─────────────────────────┐   ┌─────────────────────────┐
│ 02 - Linux Permissions  │   │ 03 - Process Management │   │ 04 - Storage & LVM      │
│ • chmod, chown, umask   │   │ • PID, signals (SIGKILL)│   │ • Physical Volumes (PV) │
│ • SUID, SGID, Sticky Bit│   │ • /proc/[PID] internals │   │ • Volume Groups (VG)    │
│ • POSIX ACLs (getfacl)  │   │ • systemd service units │   │ • Logical Volumes (LV)  │
└────────────┬────────────┘   └────────────┬────────────┘   └────────────┬────────────┘
             │                             │                             │
             └─────────────────────────────┼─────────────────────────────┘
                                           ▼
                              ┌─────────────────────────┐
                              │ 05 - Container Runtime  │
                              │ • Linux Namespaces      │
                              │ • Control Groups (cgroup│
                              │ • Chroot & Pivot_root   │
                              │ • OverlayFS Storage     │
                              └─────────────────────────┘
```

---

## 🔗 Deeply Connected Concepts

### 1. Linux File Permissions & Ownership
- **Why it connects:** Every directory in the FHS has strict default permissions (e.g., `/etc/shadow` is `0640` or `0000`, `/tmp` is `1777` with the sticky bit).
- **Next to study:** Understanding numeric and symbolic permissions (`chmod 755`), ownership (`chown`), and default file creation masks (`umask 022`).

### 2. Logical Volume Manager (LVM)
- **Why it connects:** In enterprise architectures, directories like `/var`, `/home`, and `/data` are rarely raw partitions—they are LVM logical volumes that can be expanded dynamically without unmounting the filesystem.
- **Next to study:** `pvcreate`, `vgextend`, `lvextend`, and online filesystem resizing with `xfs_growfs` and `resize2fs`.

### 3. Container Mechanics (Docker, Containerd, Kubernetes)
- **Why it connects:** Containers achieve filesystem isolation by creating a new **Mount Namespace** and executing `chroot` or `pivot_root` to make an isolated directory appear as `/` to the containerized application.
- **Next to study:** Docker storage drivers, OverlayFS upper/lower layers, container volume mounts, and Kubernetes PersistentVolumeClaims (PVC).

---

## 📚 Authoritative References & Recommended Reading
1. **Linux Foundation FHS 3.0 Specification:** [Filesystem Hierarchy Standard Official Doc](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html)
2. **Linux Manual Pages:**
   - `man 7 hier` — Description of the filesystem hierarchy.
   - `man 5 fstab` — Static information about the filesystems.
   - `man 8 mount` — Mount a filesystem.
   - `man 5 proc` — Process information pseudo-filesystem.
3. **Freedesktop.org:** The Case for Merged `/usr` (UsrMerge documentation).
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - Quick Revision](./13-Quick-Revision.md) | [README](./README.md) | — |
