# 02 — GRUB2 Configuration and Kernel Parameters

GRUB2 (Grand Unified Bootloader) is the primary bootloader for modern Linux systems.

---

## 1. Modifying Bootloader Configuration

Never edit `/boot/grub/grub.cfg` directly! It is automatically generated from `/etc/default/grub` and scripts in `/etc/grub.d/`.

Edit `/etc/default/grub`:
```text
GRUB_TIMEOUT=5
GRUB_DEFAULT=0
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash net.ifnames=0 biosdevname=0"
```

Rebuild configuration:
- **Debian / Ubuntu:**
  ```bash
  sudo update-grub
  ```
- **RHEL / Rocky / CentOS:**
  ```bash
  sudo grub2-mkconfig -o /boot/grub2/grub.cfg
  ```

---

## 2. Emergency Kernel Boot Parameters

At the GRUB boot menu, pressing `e` allows you to temporarily edit the kernel boot line:

| Parameter | Action |
| :--- | :--- |
| `single` or `1` | Boots into single-user maintenance mode (SysV/systemd rescue). |
| `systemd.unit=rescue.target` | Boots into systemd rescue mode (root filesystem mounted, basic services). |
| `systemd.unit=emergency.target`| Boots into minimal emergency shell with root mounted **read-only**. |
| `rd.break` | (RHEL/CentOS) Interrupts bootloader right before pivoting from initramfs to real root. |
| `init=/bin/bash` | Bypasses systemd completely and launches an interactive bash shell as PID 1! |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Linux Boot Process](./01-Linux-Boot-Process-Deep-Dive-UEFI-GRUB2-Initrd.md) | [README](./README.md) | [03 - Initramfs & Dracut](./03-Initramfs-and-Dracut-Internals.md) |
