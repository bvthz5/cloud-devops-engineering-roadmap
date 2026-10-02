# 06 — Recovering from fstab Errors and Kernel Panics

A single typographical error in `/etc/fstab` (such as a misspelled UUID or unmounted disk without `nofail`) will cause systemd to fail mount checks and halt into an emergency shell.

---

## 1. Fixing `/etc/fstab` in Emergency Mode

1. At the emergency shell prompt, the root filesystem is typically mounted read-only (`ro`).
2. Remount root as read-write:
   ```bash
   mount -o remount,rw /
   ```
3. Edit `/etc/fstab` and fix the syntax or comment out the broken mount:
   ```bash
   nano /etc/fstab
   ```
   > [!TIP]
   > Always add the `nofail` option to non-critical secondary disks (e.g. `UUID=... /data ext4 defaults,nofail 0 2`) so the server will boot even if the disk fails to attach!
4. Test all fstab mounts without rebooting:
   ```bash
   mount -a
   ```
5. Resume normal boot:
   ```bash
   systemctl default
   ```

---

## 2. Kernel Panic Diagnostics

A **Kernel Panic** occurs when the kernel encounters an unrecoverable fatal error. Common causes:
- Corrupted initramfs image.
- Storage controller driver missing from initramfs.
- `root=` boot parameter pointing to non-existent UUID or disk.

**Resolution:**
Reboot into an earlier kernel version from the GRUB "Advanced options for Ubuntu / Rocky" menu, then rebuild the initramfs using `update-initramfs -u` or `dracut -f`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Resetting Root Password](./05-Resetting-Lost-Root-Password-via-GRUB.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
