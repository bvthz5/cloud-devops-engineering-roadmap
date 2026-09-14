# 4. Ubuntu Partition Layouts & Recommended Sizing

## 1. Desktop / Laptop Standard (UEFI + LVM)
| Partition | Mount Point | Filesystem | Recommended Size | Notes |
| --- | --- | --- | --- | --- |
| `/dev/sda1` | `/boot/efi` | FAT32 | 512 MB | Required for UEFI |
| `/dev/sda2` | `/boot` | ext4 | 1 GB | Kernel images & initramfs |
| `/dev/sda3` | LVM (`vg0`) | LVM PV | Remaining Disk | Flexible volume management |

## 2. Production Enterprise Cloud Server (LVM)
- **`/` (Root):** 20 GB – 50 GB (OS binaries, software packages)
- **`/var`:** 30 GB – 100 GB+ (Logs, application caches, databases)
- **`/var/log`:** Dedicated 10 GB – 20 GB partition (prevents log spikes from filling root `/`)
- **`/home`:** Dedicated partition (preserves user data across OS reinstalls)
- **`/tmp`:** 5 GB – 10 GB (mounted with `nodev,nosuid,noexec` for security)
- **Swap:** 2 GB – 8 GB (or swapfile)
