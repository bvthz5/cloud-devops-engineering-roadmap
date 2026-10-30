# 04 — Filesystem Internals: ext4, XFS, and ZFS

A filesystem organizes raw blocks into directories, files, permissions, and metadata. Choosing the right filesystem directly impacts database I/O performance and crash recovery speed.

---

## 1. Filesystem Comparison Matrix

| Feature | ext4 (Fourth Extended FS) | XFS | ZFS / Btrfs |
| :--- | :--- | :--- | :--- |
| **Default On** | Debian, Ubuntu | RHEL, Rocky, CentOS | Enterprise Storage, TrueNAS |
| **Max File Size**| 16 Terabytes | 8 Exabytes | 16 Exabytes |
| **Shrink Support**| Yes (Offline only) | **No** (Cannot be shrunk!) | No |
| **Online Growth**| Yes (`resize2fs`) | Yes (`xfs_growfs`) | Yes |
| **Architecture** | Traditional Inode Allocation | Allocation Groups (High Parallel I/O) | Copy-on-Write (CoW), Merkle Tree |
| **Data Integrity**| Journaling only | Journaling only | Checksumming (Self-healing bit rot) |

---

## 2. The Linux Inode System

An **Inode** (index node) stores metadata about a file:
- File size, permissions, owner, group
- Timestamps (atime, mtime, ctime)
- Pointers to data blocks on disk
- **Note:** Inodes do NOT store the filename or file contents (filenames live in directory data blocks).

```bash
# Check available Inodes (Filesystems can run out of inodes even with 500GB disk space free!)
df -i

# Inspect specific file inode number and metadata
stat /etc/passwd
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Software RAID with mdadm](./03-Software-RAID-with-mdadm.md) | [Index](../../../README.md) | [05 - Network Storage NFS and iSCSI in Cloud →](./05-Network-Storage-NFS-and-iSCSI-in-Cloud.md) |
