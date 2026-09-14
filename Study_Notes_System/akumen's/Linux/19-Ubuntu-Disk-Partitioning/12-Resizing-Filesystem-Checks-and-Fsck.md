# 12. Filesystem Resizing & Integrity Checks (`fsck`)

## Filesystem Resizing Guide

### Resizing `ext4` Filesystems
```bash
# 1. Resize underlying partition or LVM Logical Volume first
sudo lvextend -L +10G /dev/vg_storage/lv_data

# 2. Expand ext4 filesystem (can be executed while mounted online)
sudo resize2fs /dev/vg_storage/lv_data
```

### Resizing `XFS` Filesystems
```bash
# XFS filesystems can only be expanded (cannot be shrunk!)
sudo xfs_growfs /mnt/data
```

## Filesystem Integrity Check (`fsck`)

> **CRITICAL RULE:** NEVER run `fsck` on a mounted filesystem! Unmount the target volume first to prevent filesystem corruption.

```bash
# Unmount partition
sudo umount /dev/sdb1

# Run interactive filesystem check & repair
sudo fsck -fy /dev/sdb1
```
