# 08 — Storage Troubleshooting Guide & Runbook

---

## 1. Recovering from Read-Only Filesystems

When Linux detects disk I/O errors or corrupted metadata, it automatically remounts the filesystem as **Read-Only (`ro`)** to prevent permanent data corruption.

```bash
# 1. Verify read-only state
mount | grep 'ro,'

# 2. Check kernel ring buffer for I/O errors
dmesg -T | grep -E -i 'error|fail|corrupt|scsi|nvme'

# 3. If uncorrupted, attempt remounting read-write
sudo mount -o remount,rw /

# 4. If metadata is damaged, take offline and repair:
# For ext4:
sudo fsck.ext4 -y /dev/sda1
# For XFS:
sudo xfs_repair /dev/sda1
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
