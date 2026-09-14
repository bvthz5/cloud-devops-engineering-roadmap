# 15. Hands-On Partitioning & LVM Lab

## Objective
Create a GPT partition table, format an `ext4` filesystem, set up permanent UUID mounting in `/etc/fstab`, and expand a Logical Volume.

## Step-by-Step Lab Execution

1. **Identify Target Secondary Disk:**
   ```bash
   lsblk
   # Target disk: /dev/sdb (20 GB)
   ```

2. **Create GPT Partition Table & Partition:**
   ```bash
   sudo parted /dev/sdb mklabel gpt
   sudo parted /dev/sdb mkpart primary ext4 1MiB 100%
   ```

3. **Format Filesystem:**
   ```bash
   sudo mkfs.ext4 -L "DataStore" /dev/sdb1
   ```

4. **Configure Mount Point & `/etc/fstab`:**
   ```bash
   sudo mkdir -p /mnt/datastore
   UUID=$(sudo blkid -s UUID -o value /dev/sdb1)
   echo "UUID=$UUID  /mnt/datastore  ext4  defaults,noatime  0  2" | sudo tee -a /etc/fstab
   ```

5. **Test Mount:**
   ```bash
   sudo mount -a
   df -h /mnt/datastore
   ```
