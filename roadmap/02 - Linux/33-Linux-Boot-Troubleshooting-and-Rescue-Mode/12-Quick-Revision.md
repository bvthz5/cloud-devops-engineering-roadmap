# 12 — Quick Revision Cheat Sheet: Boot Troubleshooting

---

## 1. Emergency Commands Reference

```bash
# Remount Root Filesystem Read-Write
mount -o remount,rw /

# Update Bootloader Config
sudo update-grub                              # Ubuntu/Debian
sudo grub2-mkconfig -o /boot/grub2/grub.cfg  # RHEL/Rocky

# Rebuild Initramfs
sudo update-initramfs -u                      # Ubuntu/Debian
sudo dracut -f                                # RHEL/Rocky

# chroot Mount Sequence
sudo mount /dev/sda1 /mnt
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
sudo chroot /mnt
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple Choice Questions](./11-MCQ.md) | [README](./README.md) | [Linux Roadmap Index](../README.md) |
