# 22. Storage Command Cheat Sheet

```bash
# List & Inspect Storage
lsblk -f                               # List block devices & filesystems
sudo blkid                             # List partition UUIDs
df -h                                  # Check filesystem space
df -i                                  # Check inode usage
sudo du -sh /var/*                     # Check directory disk usage

# Partitioning & Formatting
sudo fdisk /dev/sdb                    # Partition MBR/GPT disk
sudo parted /dev/sdb mklabel gpt       # Create GPT table
sudo mkfs.ext4 -L "Data" /dev/sdb1    # Format ext4
sudo mkfs.xfs -f /dev/sdb2             # Format XFS
sudo mkfs.vfat -F 32 /dev/sdb3         # Format EFI FAT32

# Mounting & Swap
sudo mount -a                          # Test /etc/fstab entries
sudo swapon --show                     # View active swap
sudo swapon /swapfile                  # Activate swapfile

# LVM & Resizing
sudo pvcreate /dev/sdb1                # Initialize PV
sudo vgcreate vg0 /dev/sdb1            # Create VG
sudo lvcreate -L 20G -n lv0 vg0        # Create LV
sudo lvextend -L +10G /dev/vg0/lv0     # Extend LV
sudo resize2fs /dev/vg0/lv0            # Expand ext4 online
sudo xfs_growfs /mountpoint            # Expand XFS online
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [21 - Related DevOps Storage Topics](./21-Related-DevOps-Storage-Topics.md) | [README](./README.md) | [23 - External References and Documentation](./23-External-References-and-Documentation.md) |
