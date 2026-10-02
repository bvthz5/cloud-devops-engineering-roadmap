# 02 - The `/boot` Directory

The `/boot` directory contains all critical files required for booting the Linux operating system. It holds the Linux kernel binaries, initial RAM disk images, bootloader configuration files, and kernel symbol maps.

---

## 📂 Key Contents of `/boot`

| File / Subdirectory | Description & Purpose | Example Filename |
| :--- | :--- | :--- |
| `vmlinuz-*` | The compressed executable Linux Kernel file loaded into memory by GRUB during boot. | `vmlinuz-5.15.0-88-generic` |
| `initrd.img-*` / `initramfs-*` | Initial RAM Disk image containing essential drivers (RAID, LVM, NVMe, Ext4/XFS) needed to mount `/`. | `initrd.img-5.15.0-88-generic` |
| `grub/` or `grub2/` | Directory containing GRUB bootloader configuration files, modules, and menu themes. | `/boot/grub/grub.cfg` |
| `System.map-*` | Kernel symbol table mapping function names to memory addresses (used for kernel debugging). | `System.map-5.15.0-88-generic` |
| `config-*` | The build configuration settings used when compiling the installed kernel binary. | `config-5.15.0-88-generic` |
| `efi/` | Mount point for the EFI System Partition (ESP) on UEFI systems (FAT32 partition). | `/boot/efi/EFI/ubuntu/grubx64.efi` |

---

## ⚙️ Why `/boot` is Often a Separate Partition

In production Linux deployments and enterprise servers, `/boot` is frequently created as a small, separate physical partition (typically 512 MB to 2 GB):

1. **Bootloader Access:** Legacy BIOS/GRUB bootloaders need to read kernel images before complex LVM volumes, software RAID arrays, or encrypted drives (LUKS) are unlocked.
2. **Filesystem Compatibility:** `/boot` often uses simpler filesystems (like Ext4 or FAT32/vfat for UEFI) even if the root filesystem (`/`) uses advanced systems like XFS, Btrfs, or ZFS.
3. **Security:** Marking `/boot` as read-only (`ro`) in `/etc/fstab` prevents unauthorized rootkit modifications to kernel binaries.

---

## 🛠️ Essential Commands for `/boot`

```bash
# View installed kernel images and boot files
ls -lh /boot

# Check disk space usage on /boot (Crucial to prevent full boot partition during kernel updates)
df -h /boot

# Remove old unused kernel packages (Debian/Ubuntu)
sudo apt autoremove --purge
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Filesystem Root](./01-Filesystem-Root.md) | [README](./README.md) | [03 - usr](./03-usr.md) |
