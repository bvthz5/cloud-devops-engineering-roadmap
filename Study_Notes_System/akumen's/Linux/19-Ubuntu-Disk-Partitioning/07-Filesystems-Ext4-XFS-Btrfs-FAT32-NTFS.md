# 7. Linux Filesystems: ext4, XFS, Btrfs, FAT32, NTFS

## Filesystem Comparison Matrix

| Filesystem | Max File Size | Max Volume Size | Key Features & Best Use Cases |
| --- | --- | --- | --- |
| **ext4** | 16 TB | 1 EB | Default standard for Ubuntu; stable, journaled, reliable, widely supported. |
| **XFS** | 8 EB | 8 EB | Enterprise default (RHEL); high scalability for parallel I/O, large files & databases. Cannot be shrunk. |
| **Btrfs** | 16 EB | 16 EB | Copy-on-Write (CoW), built-in RAID, subvolumes, instant snapshots, checksumming. |
| **FAT32** | 4 GB | 2 TB | Universal cross-platform compatibility; required for EFI System Partitions (`/boot/efi`). |
| **NTFS** | 16 TB | 8 PB | Proprietary Windows filesystem; read/write supported in Linux via `ntfs-3g` / `ntfs3` kernel driver. |

## Formatting Commands

```bash
# Format partition as ext4
sudo mkfs.ext4 -L "DataVolume" /dev/sdb1

# Format partition as XFS
sudo mkfs.xfs -f -L "DatabaseVol" /dev/sdb2

# Format partition as FAT32 (vfat) for EFI
sudo mkfs.vfat -F 32 /dev/sdb3
```
