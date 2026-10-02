# Module 28: Advanced Storage, LVM, RAID, and Filesystems

Storage is the foundation of stateful cloud applications, databases, and container clusters. Cloud engineers and SREs frequently encounter production emergencies involving disk space exhaustion, resizing persistent volumes without downtime, configuring high-availability software RAID, and diagnosing corrupted filesystems.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Understand Linux storage hierarchy: Block devices, partitions (GPT/MBR), and UUIDs.
- Master **LVM (Logical Volume Manager)**: Physical Volumes (PV), Volume Groups (VG), and Logical Volumes (LV).
- Perform zero-downtime **online volume and filesystem expansion** (`resize2fs`, `xfs_growfs`).
- Implement software RAID using **`mdadm`** (RAID 0, 1, 5, 10) with hot spares and disk recovery.
- Compare enterprise filesystems: **ext4, XFS, Btrfs, and ZFS**.
- Mount and troubleshoot network storage (**NFSv4**, AWS EFS, Azure Files).
- Recover from disk emergencies: Read-only filesystems, inode exhaustion (`df -i`), and superblock corruption.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Storage Architecture & Block Devices](./01-Storage-Architecture-Block-Devices-and-Partitions.md) | NVMe, SATA, GPT vs MBR, `lsblk`, `fdisk`, `parted`, and UUIDs | ✅ Complete |
| 02 | [LVM Architecture Deep Dive](./02-LVM-Logical-Volume-Manager-Architecture.md) | PVs, VGs, LVs, physical extents, and dynamic storage pooling | ✅ Complete |
| 03 | [Software RAID with mdadm](./03-Software-RAID-with-mdadm.md) | RAID 0, 1, 5, 10 architectures, hot spare failover, and rebuilding | ✅ Complete |
| 04 | [Filesystem Internals: ext4, XFS, ZFS](./04-Filesystem-Internals-ext4-XFS-and-Btrfs-ZFS.md) | Inodes, superblocks, journaling, copy-on-write, and compression | ✅ Complete |
| 05 | [Network Storage (NFS & Cloud Mounts)](./05-Network-Storage-NFS-and-iSCSI-in-Cloud.md) | NFSv4 exports, mount options (`noatime`, `rsize`, `wsize`), and autofs | ✅ Complete |
| 06 | [Online Expansion & Maintenance](./06-Online-Filesystem-Expansion-and-Maintenance.md) | Extending cloud disks, LVM growth, `xfs_growfs`, and `resize2fs` | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | 100% full disk emergency triage, live EBS volume resizing, and snapshots | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Inode exhaustion, read-only remounts, `fsck` repair, and dirty pages | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical storage interview questions for DevOps, SRE, and Cloud | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Build LVM from scratch, perform live XFS expansion, simulate RAID failure | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Complete command cheat sheet for storage, LVM, and filesystem recovery | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 27: systemd & Journald](../27-systemd-Service-Management-and-Journald/README.md) | [Linux Roadmap Index](../README.md) | [01 - Storage Architecture](./01-Storage-Architecture-Block-Devices-and-Partitions.md) |
