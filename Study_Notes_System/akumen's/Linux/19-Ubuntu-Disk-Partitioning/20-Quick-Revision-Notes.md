# 20. Quick Revision Notes

- **Partition Tables:** MBR (Legacy, max 2TB, 4 primary) | GPT (Modern, 128 partitions, >2TB support).
- **UEFI Boot:** ESP partition formatted FAT32 mounted at `/boot/efi`.
- **Filesystems:** `ext4` (Ubuntu default), `XFS` (High performance/cannot shrink), `Btrfs` (CoW/Snapshots).
- **fstab Syntax:** `UUID  MountPoint  FSType  Options  Dump  Pass`.
- **LVM Layers:** Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV).
- **fsck Warning:** Never run `fsck` on a mounted filesystem!
