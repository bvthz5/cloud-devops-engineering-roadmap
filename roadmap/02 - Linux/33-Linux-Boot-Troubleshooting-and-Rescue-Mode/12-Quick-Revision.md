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
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Section (03 - Networking) →](../../03%20-%20Networking/01-OSI-and-TCPIP-Models/01-OSI-7-Layer-Reference-Model.md) |
