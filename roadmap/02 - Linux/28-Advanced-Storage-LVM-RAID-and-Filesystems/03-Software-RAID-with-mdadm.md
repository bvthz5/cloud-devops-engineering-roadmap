# 03 — Software RAID with mdadm

RAID (Redundant Array of Independent Disks) combines multiple physical disk drives into a single logical unit for redundancy, performance, or both. In Linux, software RAID is managed via the kernel Multiple Device (`md`) driver and the `mdadm` CLI.

---

## 1. RAID Level Comparison

| RAID Level | Minimum Disks | Data Protection | Read Speed | Write Speed | Storage Efficiency |
| :--- | :---: | :--- | :---: | :---: | :---: |
| **RAID 0** (Striping) | 2 | **None** (1 disk failure = total data loss) | 2x | 2x | 100% |
| **RAID 1** (Mirroring) | 2 | Can survive 1 disk failure | 2x | 1x | 50% |
| **RAID 5** (Distributed Parity)| 3 | Can survive 1 disk failure | High | Slower (parity calc)| (N-1)/N |
| **RAID 10** (1+0 Striped Mirrors)| 4 | Can survive up to 2 disk failures (1 per mirror)| Very High | High | 50% |

---

## 2. Managing Software RAID with `mdadm`

```bash
# Create a RAID 1 mirrored array using two disks
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc

# Monitor active RAID synchronization status
cat /proc/mdstat

# Save configuration to ensure it persists across reboots
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
sudo update-initramfs -u

# Simulate disk failure (for disaster recovery drill)
sudo mdadm /dev/md0 --fail /dev/sdb

# Remove the failed disk
sudo mdadm /dev/md0 --remove /dev/sdb

# Add a replacement disk to trigger automatic resynchronization
sudo mdadm /dev/md0 --add /dev/sdd
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - LVM Architecture](./02-LVM-Logical-Volume-Manager-Architecture.md) | [README](./README.md) | [04 - Filesystem Internals](./04-Filesystem-Internals-ext4-XFS-and-Btrfs-ZFS.md) |
