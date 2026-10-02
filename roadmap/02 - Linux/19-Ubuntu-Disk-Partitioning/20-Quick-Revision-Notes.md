# 20. Quick Revision Notes

- **Partition Tables:** MBR (Legacy, max 2TB, 4 primary) | GPT (Modern, 128 partitions, >2TB support).
- **UEFI Boot:** ESP partition formatted FAT32 mounted at `/boot/efi`.
- **Filesystems:** `ext4` (Ubuntu default), `XFS` (High performance/cannot shrink), `Btrfs` (CoW/Snapshots).
- **fstab Syntax:** `UUID  MountPoint  FSType  Options  Dump  Pass`.
- **LVM Layers:** Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV).
- **fsck Warning:** Never run `fsck` on a mounted filesystem!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [19 - Hands On Scenario Challenge](./19-Hands-On-Scenario-Challenge.md) | [README](./README.md) | [21 - Related DevOps Storage Topics](./21-Related-DevOps-Storage-Topics.md) |
