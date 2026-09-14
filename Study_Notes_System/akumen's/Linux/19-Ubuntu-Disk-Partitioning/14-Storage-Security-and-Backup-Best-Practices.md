# 14. Storage Security & Backup Best Practices

## Security Best Practices
1. **Hardened Mount Options:** Mount temporary/user directories (`/tmp`, `/var/tmp`, `/dev/shm`) with `nodev,nosuid,noexec`.
2. **Secure `/etc/fstab` Permissions:** Ensure `/etc/fstab` is owned by `root:root` with mode `644`.
3. **LUKS Key Storage:** Never store unencrypted LUKS passphrases on local boot partitions.

## Backup Strategies

### 1. `rsync` Backup
```bash
# Incremental file synchronization preserving permissions & timestamps
rsync -aAXv --delete /data/ /backup/data/
```

### 2. `dd` Disk Imaging (Low-Level Sector Copy)
```bash
# Backup raw disk image to compressed file
sudo dd if=/dev/sda status=progress | gzip > /backup/disk_image.img.gz
```
