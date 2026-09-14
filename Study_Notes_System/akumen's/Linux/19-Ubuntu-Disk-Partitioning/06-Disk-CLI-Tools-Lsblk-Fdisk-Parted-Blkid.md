# 6. Disk CLI Tools: `lsblk`, `fdisk`, `parted`, `blkid`

## 1. `lsblk` — List Block Devices
```bash
# Display block devices in tree format with filesystems & mount points
lsblk -f

# Include device size, permissions, and device type
lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,UUID,MODEL
```

## 2. `blkid` — Query Block Device Attributes (UUID & Filesystem)
```bash
# Display UUIDs and filesystem types for all block devices
sudo blkid

# Query specific partition
sudo blkid /dev/sda1
```

## 3. `fdisk` — Partition Table Manipulator (MBR & GPT)
```bash
# Open disk in interactive fdisk shell
sudo fdisk /dev/sdb

# Useful fdisk interactive commands:
# p - Print partition table
# n - Add a new partition
# d - Delete a partition
# t - Change partition type
# w - Write changes to disk and exit
# q - Quit without saving changes
```

## 4. `parted` — Advanced Partitioning Tool (Supports Scripting & >2TB GPT)
```bash
# Print partition table
sudo parted /dev/sdb print

# Create a new GPT partition table
sudo parted /dev/sdb mklabel gpt

# Create a 10 GB primary ext4 partition
sudo parted /dev/sdb mkpart primary ext4 1MiB 10GiB
```
