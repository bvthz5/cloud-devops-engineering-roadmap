# 3. Legacy BIOS vs UEFI & EFI System Partition (ESP)

## Firmware Comparison

### Legacy BIOS (Basic Input/Output System)
- Executes 16-bit real mode code from MBR Sector 0.
- Lacks modern driver support, limited to slow boot speeds.
- Boots directly into GRUB Stage 1 written in MBR gap.

### UEFI (Unified Extensible Firmware Interface)
- Modern 64-bit firmware with graphical interface and secure boot support.
- Reads files directly from a FAT32 filesystem stored on a dedicated **EFI System Partition (ESP)**.

## EFI System Partition (ESP) Requirements
- **Filesystem Format:** Must be `FAT32` (`vfat`).
- **Mount Point in Ubuntu:** `/boot/efi`
- **Recommended Size:** `512 MB` to `1 GB`.
- **Partition Flag:** `boot, esp` (Type GUID: `C12A7328-F81F-11D2-BA4B-00A0C93EC93B`).
- **Directory Structure:**
```text
  /boot/efi/EFI/ubuntu/
  ├── shimx64.efi   (Secure Boot signed bootloader loader)
  ├── grubx64.efi   (GRUB2 Bootloader binary)
  └── grub.cfg      (GRUB configuration file pointer)
```
