# 03 — Initramfs and Dracut Internals

The **Initramfs** (Initial RAM Filesystem) is a gzipped cpio archive containing minimal kernel drivers, utilities, and scripts needed to find and mount the real root partition.

---

## 1. Inspecting Initramfs

```bash
# Debian / Ubuntu: List modules and files inside initramfs
lsinitramfs /boot/initrd.img-$(uname -r) | grep -i nvme

# RHEL / Rocky: Inspect with dracut
lsinitrd /boot/initramfs-$(uname -r).img
```

---

## 2. Rebuilding Initramfs

If you install new storage drivers or alter LVM configurations:
- **Debian / Ubuntu:**
  ```bash
  sudo update-initramfs -u -k all
  ```
- **RHEL / Rocky / CentOS:**
  ```bash
  sudo dracut -f /boot/initramfs-$(uname -r).img $(uname -r)
  ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - GRUB2 Configuration and Kernel Parameters](./02-GRUB2-Configuration-and-Kernel-Parameters.md) | [Index](../../../README.md) | [04 - Emergency Mode vs Rescue Mode →](./04-Emergency-Mode-vs-Rescue-Mode.md) |
