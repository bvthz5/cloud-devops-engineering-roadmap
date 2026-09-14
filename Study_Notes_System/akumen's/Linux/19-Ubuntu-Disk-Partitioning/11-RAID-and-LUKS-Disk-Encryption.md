# 11. RAID Arrays & LUKS Disk Encryption

## 1. RAID (Redundant Array of Independent Disks)
- **RAID 0 (Striping):** High performance, zero fault tolerance (Data loss if 1 disk fails).
- **RAID 1 (Mirroring):** Exact copy on 2 disks, 50% capacity, 1 disk failure tolerance.
- **RAID 5 (Striping + Parity):** Requires ≥3 disks, 1 disk failure tolerance.
- **RAID 10 (Striped Mirrors):** High performance & fault tolerance, requires ≥4 disks.

Management tool: `mdadm`

## 2. LUKS (Linux Unified Key Setup) Disk Encryption
LUKS provides full-disk block-level encryption protecting data at rest.

### Encrypting a Partition with LUKS:
```bash
# Format partition with LUKS encryption
sudo cryptsetup luksFormat /dev/sdb1

# Open encrypted device (maps to /dev/mapper/secure_data)
sudo cryptsetup open /dev/sdb1 secure_data

# Format mapped decrypted device
sudo mkfs.ext4 /dev/mapper/secure_data

# Mount filesystem
sudo mount /dev/mapper/secure_data /mnt/secure
```
