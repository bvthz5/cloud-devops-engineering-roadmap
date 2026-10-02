# 12 — Quick Revision Cheat Sheet: Storage & LVM

---

## 1. Storage Command Reference

```bash
# Block Device & Partitioning
lsblk -f                      # Inspect block device tree & UUIDs
sudo blkid                    # Print block device attributes
sudo parted /dev/sdb          # GPT partitioning

# LVM Management
sudo pvcreate /dev/sdb        # Initialize PV
sudo vgcreate vg01 /dev/sdb   # Create VG
sudo lvcreate -L 20G -n lv01 vg01 # Create LV
sudo lvextend -l +100%FREE -r /dev/vg01/lv01 # Grow LV + FS online!

# Filesystem Growth
sudo resize2fs /dev/vg01/lv01 # Grow ext4
sudo xfs_growfs /mount/point  # Grow XFS

# Phantom Disk Space Cleanup
sudo lsof +L1                 # Find unlinked open deleted files
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (29-Linux-Performance-Tuning-and-Observability) →](../29-Linux-Performance-Tuning-and-Observability/01-Linux-Performance-Methodologies-USE-and-RED.md) |
