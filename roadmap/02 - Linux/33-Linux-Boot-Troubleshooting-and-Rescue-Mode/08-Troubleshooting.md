# 08 — Boot Troubleshooting Guide & chroot Runbook

---

## 1. The Universal `chroot` Rescue Protocol

When a server cannot boot on its own, attach its disk to a working recovery instance (or boot from a Live ISO) and chroot into the damaged root environment:

```bash
# 1. Mount the damaged system's root partition
sudo mount /dev/sdb1 /mnt

# 2. Mount virtual kernel filesystems into the chroot jail (MANDATORY!)
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
sudo mount --bind /run /mnt/run

# 3. Enter the damaged system environment as root!
sudo chroot /mnt

# 4. Now perform any maintenance:
passwd root                    # Reset password
update-initramfs -u            # Rebuild initramfs
grub-install /dev/sdb          # Reinstall bootloader
update-grub                    # Rebuild GRUB config

# 5. Cleanly exit and unmount
exit
sudo umount -R /mnt
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
