# 10. LVM: Physical Volumes, Volume Groups & Logical Volumes

Logical Volume Management (LVM) abstracts physical storage into dynamic, resizable volumes.

## LVM Architecture

```text
[ Physical Hard Drive /dev/sdb ]   [ Physical Hard Drive /dev/sdc ]
               │                                  │
               ▼                                  ▼
    Physical Volume (PV)               Physical Volume (PV)
         /dev/sdb1                          /dev/sdc1
               └─────────────────┬────────────────┘
                                 ▼
                       Volume Group (VG)
                          "vg_storage"
                                 │
                 ┌───────────────┴───────────────┐
                 ▼                               ▼
       Logical Volume (LV)             Logical Volume (LV)
           "lv_web"                         "lv_db"
                 │                               │
                 ▼                               ▼
           ext4 Filesystem                XFS Filesystem
              /var/www                       /var/lib/mysql
```

## LVM Setup Commands

```bash
# Step 1: Initialize Physical Volumes (PV)
sudo pvcreate /dev/sdb1 /dev/sdc1

# Step 2: Create Volume Group (VG)
sudo vgcreate vg_storage /dev/sdb1 /dev/sdc1

# Step 3: Create Logical Volume (LV)
sudo lvcreate -L 50G -n lv_web vg_storage

# Step 4: Format & Mount Logical Volume
sudo mkfs.ext4 /dev/vg_storage/lv_web
sudo mkdir -p /var/www
sudo mount /dev/vg_storage/lv_web /var/www

# Step 5: Dynamically Extend Logical Volume (Online Resize)
sudo lvextend -L +20G /dev/vg_storage/lv_web
sudo resize2fs /dev/vg_storage/lv_web
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Swap Partitions and Swap Files](./09-Swap-Partitions-and-Swap-Files.md) | [README](./README.md) | [11 - RAID and LUKS Disk Encryption](./11-RAID-and-LUKS-Disk-Encryption.md) |
