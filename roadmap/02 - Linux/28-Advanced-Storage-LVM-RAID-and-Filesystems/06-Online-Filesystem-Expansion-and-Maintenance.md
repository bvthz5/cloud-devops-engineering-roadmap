# 06 — Online Filesystem Expansion and Maintenance

In cloud infrastructure, applications cannot be stopped for hours just to expand a database volume. Resizing disks and filesystems online without unmounting is an essential skill.

---

## 1. Step-by-Step Online Cloud Disk Expansion Runbook

When you increase a disk from 50GB to 100GB in AWS EBS, GCP Persistent Disk, or VMware:

### Step 1: Rescan the SCSI/NVMe Bus
Tell the Linux kernel to re-read the disk size without rebooting:
```bash
# For SCSI/SATA:
echo 1 | sudo tee /sys/class/block/sda/device/rescan

# For NVMe (Automatic in modern kernels, or run):
sudo nvme ns-rescan /dev/nvme0
```

### Step 2: Resize the Partition Table (if partitioned)
```bash
# Expand partition 1 of /dev/sda to fill available space
sudo growpart /dev/sda 1
```

### Step 3: Expand LVM (if using LVM)
```bash
# Resize Physical Volume
sudo pvresize /dev/sda1

# Extend Logical Volume to consume 100% of remaining free space
sudo lvextend -l +100%FREE /dev/vg_production/lv_app
```

### Step 4: Expand the Filesystem Online (Live Mount!)
```bash
# If ext4:
sudo resize2fs /dev/vg_production/lv_app

# If XFS (Target MUST be the MOUNT POINT, not the device path!):
sudo xfs_growfs /opt/app
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Network Storage](./05-Network-Storage-NFS-and-iSCSI-in-Cloud.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
