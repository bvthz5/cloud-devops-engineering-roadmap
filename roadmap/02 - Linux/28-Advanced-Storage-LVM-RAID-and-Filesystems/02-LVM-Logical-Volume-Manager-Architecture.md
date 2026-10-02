# 02 — LVM (Logical Volume Manager) Architecture

Traditional disk partitions have static boundaries. If `/var` runs out of space, traditional partitions require taking the server offline, repartitioning, or moving data. **LVM** introduces an abstraction layer that pools physical storage into flexible, dynamically resizable virtual volumes.

---

## 1. The 3-Tier LVM Architecture

```text
[ Physical Disks / Partitions ]
     /dev/sdb1               /dev/sdc1
        |                       |
        v                       v
[ Physical Volumes (PV) ]   [ Physical Volumes (PV) ]
     pvcreate                pvcreate
        \                     /
         \                   /
          v                 v
    +-----------------------------+
    | Volume Group (VG)           |
    | (vg_data: e.g. 500 GB Pool) |
    | Allocated in Physical       |
    | Extents (PE, default 4MB)   |
    +-----------------------------+
             /           \
            v             v
[ Logical Volume 1 ]   [ Logical Volume 2 ]
  (lv_database)          (lv_logs)
     100 GB                 50 GB
        |                      |
     mkfs.xfs               mkfs.ext4
        |                      |
     /var/lib/mysql         /var/log
```

1. **Physical Volume (PV):** A raw block device or partition initialized with `pvcreate`.
2. **Volume Group (VG):** A storage pool combining one or more PVs into a unified allocation pool.
3. **Logical Volume (LV):** A virtual partition carved out of a VG, on which you create filesystems.

---

## 2. LVM Lifecycle Commands

```bash
# 1. Initialize Physical Volumes
sudo pvcreate /dev/sdb /dev/sdc

# 2. Create a Volume Group named 'vg_production'
sudo vgcreate vg_production /dev/sdb /dev/sdc

# 3. Create a 100GB Logical Volume named 'lv_app'
sudo lvcreate -L 100G -n lv_app vg_production

# 4. Format with XFS filesystem
sudo mkfs.xfs /dev/vg_production/lv_app

# 5. Mount the volume
sudo mkdir -p /opt/app
sudo mount /dev/vg_production/lv_app /opt/app
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Storage Architecture](./01-Storage-Architecture-Block-Devices-and-Partitions.md) | [README](./README.md) | [03 - Software RAID with mdadm](./03-Software-RAID-with-mdadm.md) |
