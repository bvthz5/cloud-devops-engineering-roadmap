# 8. `/etc/fstab` and Permanent UUID Mounting

The `/etc/fstab` file defines how disk partitions and remote filesystems are automatically mounted at boot time.

## Structure of `/etc/fstab`

`/etc/fstab` contains 6 space-separated fields per line:

```text
[Device/UUID]  [Mount Point]  [FSType]  [Mount Options]  [Dump]  [Fsck Pass]
```

### Example Entry:
```config
UUID=a1b2c3d4-e5f6-7890-abcd-1234567890ab  /mnt/data  ext4  defaults,noatime,nodev  0  2
```

## Field Breakdown
1. **Device Identifier:** Always use `UUID=` instead of `/dev/sda1` because device node names change on reboot.
2. **Mount Point:** Directory target (must exist prior to mounting).
3. **Filesystem Type:** `ext4`, `xfs`, `btrfs`, `vfat`, `swap`.
4. **Mount Options:**
   - `defaults`: Enables `rw, suid, dev, exec, auto, nouser, async`.
   - `noatime`: Disables file access time updates (improves disk performance).
   - `nodev`: Disables character/block special devices on partition.
   - `nosuid`: Blocks SUID/SGID execution (Security requirement for `/tmp` & `/var`).
   - `nofail`: Prevents system boot failure if disk is unplugged or missing!
5. **Dump (Backup):** `0` (disabled) or `1` (backup via `dump` tool).
6. **Fsck Pass (Disk Check):** `0` (do not check), `1` (Root `/` priority check), `2` (other partitions).

## Testing `/etc/fstab` Without Rebooting
```bash
# Test mounting all filesystems defined in /etc/fstab
sudo mount -a

# If mount -a reports errors, fix /etc/fstab immediately before rebooting!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Filesystems Ext4 XFS Btrfs FAT32 NTFS](./07-Filesystems-Ext4-XFS-Btrfs-FAT32-NTFS.md) | [README](./README.md) | [09 - Swap Partitions and Swap Files](./09-Swap-Partitions-and-Swap-Files.md) |
