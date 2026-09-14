# Topic 19 — Ubuntu Disk Partitioning

## Objective
Master Linux disk management, partition schemes (MBR vs GPT), boot architectures (BIOS vs UEFI/ESP), filesystems (ext4, XFS, Btrfs), mount configurations (`/etc/fstab`), Swap management, LVM (Logical Volume Management), LUKS disk encryption, and disk troubleshooting in Ubuntu/Linux environments.

## What this topic covers
- Disk, partition, filesystem, & mount point fundamentals
- MBR (Master Boot Record) vs GPT (GUID Partition Table)
- Legacy BIOS vs UEFI & EFI System Partition (ESP)
- Ubuntu storage layouts (desktop, server, production cloud)
- Manual partitioning & dual-boot considerations
- CLI storage tools: `lsblk`, `fdisk`, `parted`, `blkid`, `mount`, `df`, `du`, `findmnt`, `swapon`
- Filesystem types: `ext4`, `XFS`, `Btrfs`, `FAT32`, `NTFS`
- Permanent mounting via `/etc/fstab` & UUID resolution
- Swap partitions vs Swap files management
- LVM architecture: Physical Volumes (PV) → Volume Groups (VG) → Logical Volumes (LV)
- RAID arrays & LUKS disk encryption
- Resizing partitions (`resize2fs`, `xfs_growfs`), integrity checking (`fsck`), & recovery
- Storage performance benchmarking, I/O bottlenecks (`iostat`), & space troubleshooting
- Security hardening & backup best practices
- Hands-on partitioning labs & real-world scenario challenges
- Interview Q&A, MCQs, Command Cheat Sheet, & Reference documentation

## Recommended learning order
1. 01-Disk-Partition-Filesystem-Fundamentals.md
2. 02-MBR-vs-GPT-Partition-Tables.md
3. 03-BIOS-vs-UEFI-and-ESP-Partition.md
4. 04-Ubuntu-Partition-Layouts-and-Recommended-Sizes.md
5. 05-Manual-Installation-and-Dual-Booting.md
6. 06-Disk-CLI-Tools-Lsblk-Fdisk-Parted-Blkid.md
7. 07-Filesystems-Ext4-XFS-Btrfs-FAT32-NTFS.md
8. 08-Etc-Fstab-and-UUID-Mounting.md
9. 09-Swap-Partitions-and-Swap-Files.md
10. 10-LVM-Physical-Volumes-Volume-Groups-Logical-Volumes.md
11. 11-RAID-and-LUKS-Disk-Encryption.md
12. 12-Resizing-Filesystem-Checks-and-Fsck.md
13. 13-Storage-Performance-and-Capacity-Troubleshooting.md
14. 14-Storage-Security-and-Backup-Best-Practices.md
15. 15-Hands-On-Partitioning-Lab.md
16. 16-Troubleshooting-Partitioning-Scenarios.md
17. 17-Interview-Questions-and-Answers.md
18. 18-MCQs-and-Quick-Revision.md
19. 19-Hands-On-Scenario-Challenge.md
20. 20-Quick-Revision-Notes.md
21. 21-Related-DevOps-Storage-Topics.md
22. 22-Command-Reference-Cheat-Sheet.md
23. 23-External-References-and-Documentation.md

`SOURCE.md` preserves the original foundation and official documentation references.
