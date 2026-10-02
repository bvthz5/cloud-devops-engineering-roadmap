# Module 33: Linux Boot Troubleshooting, GRUB, and Rescue Mode

When a server crashes and refuses to boot, standard SSH access is completely unavailable. SREs, Systems Administrators, and Cloud Engineers must know how to diagnose the system via out-of-band consoles (IPMI, AWS Serial Console, GCP Serial Port, VMware VNC) to repair broken bootloaders, corrupted `/etc/fstab` entries, missing kernel drivers, and damaged filesystems.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Dissect the **5 distinct stages of the Linux boot process** (UEFI -> GRUB2 -> Kernel -> Initramfs -> systemd).
- Configure GRUB2 bootloader parameters and rebuild configs (`update-grub`, `grub2-mkconfig`).
- Inspect and regenerate the initial ramdisk (**initramfs / dracut**).
- Boot into **Rescue Mode** (`rescue.target`) and **Emergency Mode** (`emergency.target`).
- Reset forgotten root passwords using `rd.break` or `init=/bin/bash` with SELinux autorelabeling.
- Fix broken `/etc/fstab` syntax errors that halt systemd boot.
- Perform root repairs using **`chroot`** environments from Live Recovery ISOs or rescue volumes.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Linux Boot Process Deep Dive](./01-Linux-Boot-Process-Deep-Dive-UEFI-GRUB2-Initrd.md) | Firmware, GRUB2, kernel decompression, initramfs, and PID 1 handoff | ✅ Complete |
| 02 | [GRUB2 Configuration & Boot Parameters](./02-GRUB2-Configuration-and-Kernel-Parameters.md) | `/etc/default/grub`, kernel command-line arguments, and emergency targets | ✅ Complete |
| 03 | [Initramfs & Dracut Internals](./03-Initramfs-and-Dracut-Internals.md) | Temporary root filesystem, storage drivers, and rebuilding with `dracut` | ✅ Complete |
| 04 | [Emergency Mode vs Rescue Mode](./04-Emergency-Mode-vs-Rescue-Mode.md) | `rescue.target` vs `emergency.target`, single-user maintenance mode | ✅ Complete |
| 05 | [Resetting Lost Root Password via GRUB](./05-Resetting-Lost-Root-Password-via-GRUB.md) | Step-by-step console guide: `rd.break`, remounting `/sysroot`, and SELinux | ✅ Complete |
| 06 | [Recovering from fstab Errors & Kernel Panics](./06-Recovering-from-fstab-and-Kernel-Panic.md) | Fixing invalid disk UUIDs, read-only remounts, and VFS kernel panics | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Cloud VM unbootable after kernel patch; AWS serial console recovery | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | `chroot` rescue protocol, repairing grub-install, and running `fsck` | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical boot troubleshooting interview questions for SRE/DevOps | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Inspect initramfs, simulate broken fstab, and repair via emergency shell | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Complete emergency console commands, kernel parameters, and chroot steps | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 32: SSH Architecture](../32-SSH-Architecture-Key-Management-and-Tunneling/README.md) | [Linux Roadmap Index](../README.md) | [01 - Linux Boot Process](./01-Linux-Boot-Process-Deep-Dive-UEFI-GRUB2-Initrd.md) |
