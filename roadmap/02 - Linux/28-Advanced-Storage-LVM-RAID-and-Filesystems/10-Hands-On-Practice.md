# 10 — Hands-On Practice Labs: Storage & LVM

---

## Lab 1: Building LVM Storage Pools from Loopback Devices

### Objective
Create virtual block devices using loopback files to practice LVM creation, volume mounting, and live expansion without needing secondary physical disks.

```bash
# 1. Create two 1GB sparse test image files
sudo dd if=/dev/zero of=/var/tmp/disk1.img bs=1M count=1024
sudo dd if=/dev/zero of=/var/tmp/disk2.img bs=1M count=1024

# 2. Attach image files as loop devices
sudo losetup -fP /var/tmp/disk1.img
sudo losetup -fP /var/tmp/disk2.img
lsblk | grep loop

# 3. Create LVM Physical Volumes
sudo pvcreate /dev/loop0 /dev/loop1

# 4. Create Volume Group
sudo vgcreate vg_lab /dev/loop0 /dev/loop1

# 5. Create Logical Volume and format with ext4
sudo lvcreate -L 500M -n lv_test vg_lab
sudo mkfs.ext4 /dev/vg_lab/lv_test

# 6. Mount and test live online expansion!
sudo mkdir -p /mnt/lvm_lab
sudo mount /dev/vg_lab/lv_test /mnt/lvm_lab
df -h /mnt/lvm_lab

# Extend LV by 500MB and resize filesystem in one command!
sudo lvextend -L +500M -r /dev/vg_lab/lv_test
df -h /mnt/lvm_lab
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
