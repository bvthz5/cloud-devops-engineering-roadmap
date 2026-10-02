# 01 — Linux Boot Process Deep Dive: UEFI, GRUB2, and Initramfs

The journey from pressing the physical power button (or launching a cloud VM) to an interactive bash prompt involves five distinct, sequential phases.

---

## The 5 Boot Stages

```text
[ 1. Firmware Stage: BIOS / UEFI ]
- POST (Power-On Self-Test) hardware validation
- Reads NVRAM boot entries; locates EFI System Partition (ESP)
- Loads bootloader EFI binary (e.g. /EFI/ubuntu/grubx64.efi)
       ↓
[ 2. Bootloader Stage: GRUB2 ]
- Reads /boot/grub/grub.cfg
- Displays boot menu (or auto-selects default kernel)
- Loads Kernel image (vmlinuz) and Initial Ramdisk (initramfs) into memory
       ↓
[ 3. Kernel Initialization Stage ]
- Uncompresses kernel and initializes CPU, memory, and devices
- Executes kernel code: setup architecture, page tables, interrupts
       ↓
[ 4. Initramfs / Initrd Stage (Temporary Root) ]
- Kernel mounts in-memory initramfs as temporary root (/)
- Loads essential storage drivers (NVMe, SCSI, RAID, LVM)
- Mounts real root filesystem from disk onto /sysroot
- Executes pivot_root to switch to real disk root filesystem
       ↓
[ 5. systemd Initialization Stage (PID 1) ]
- Executes /sbin/init (systemd)
- Mounts filesystems defined in /etc/fstab
- Starts services in parallel based on default.target (multi-user.target)
- Spawns login prompts / SSH daemon (sshd)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - GRUB2 Configuration](./02-GRUB2-Configuration-and-Kernel-Parameters.md) |
