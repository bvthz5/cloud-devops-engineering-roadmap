# 07 - Firmware, BIOS, UEFI & System Boot Process

---

## 1. What Is Firmware?

Firmware is permanent, low-level operational software burned directly into non-volatile read-only flash memory (SPI ROM) on computer circuit boards. It provides the initial hardware initialization routines required before the operating system kernel is loaded into RAM.

---

## 2. Legacy BIOS vs. Modern UEFI

For three decades, IBM PC-compatibles relied on **BIOS (Basic Input/Output System)**. Modern computing has universally migrated to **UEFI (Unified Extensible Firmware Interface)**.

| Feature / Capability | Legacy BIOS | Modern UEFI |
|---|---|---|
| **Architecture** | 16-bit Real Mode (1 MB addressable memory) | 32-bit or 64-bit Protected Mode |
| **Partition Table Standard** | **MBR (Master Boot Record):** Max 2 TB disk size, 4 primary partitions | **GPT (GUID Partition Table):** Up to 9.4 Zettabytes, 128+ partitions |
| **Boot Speed** | Slow sequential hardware probing | Fast parallel device initialization |
| **Security** | None (vulnerable to bootkits/rootkits) | **Secure Boot:** Validates cryptographic signatures of bootloaders |
| **Storage Location** | Sector 0 of disk (512 bytes) | Dedicated **EFI System Partition (ESP)** formatted with FAT32 |

---

## 3. The End-to-End System Boot Process

Understanding the physical boot sequence is vital for bare-metal cloud provisioning (PXE boot, Terraform/Ironic, AWS i3/metal instances):

```text
┌────────────────────────────────────────────────────────┐
│ 1. Power-On & CPU Reset                                │
│ Electrical power stabilizes. CPU Program Counter sets  │
│ to hardwired reset vector (0xFFFFFFF0).                │
└───────────────────────────┬────────────────────────────┘
                            ▼
┌────────────────────────────────────────────────────────┐
│ 2. POST (Power-On Self-Test)                           │
│ Firmware tests CPU, initializes DRAM controller, checks│
│ PCIe buses, keyboard, and basic I/O integrity.         │
└───────────────────────────┬────────────────────────────┘
                            ▼
┌────────────────────────────────────────────────────────┐
│ 3. UEFI Boot Manager & Secure Boot                     │
│ Reads NVRAM boot priority. Mounts the EFI System       │
│ Partition (/boot/efi). Validates digital signature.    │
└───────────────────────────┬────────────────────────────┘
                            ▼
┌────────────────────────────────────────────────────────┐
│ 4. Bootloader (GRUB / systemd-boot)                    │
│ Loads Linux Kernel (vmlinuz) and Initial RAM Disk      │
│ (initramfs) into memory. Passes kernel parameters.     │
└───────────────────────────┬────────────────────────────┘
                            ▼
┌────────────────────────────────────────────────────────┐
│ 5. Kernel Initialization & PID 1                       │
│ Kernel detects hardware, mounts root filesystem (/),   │
│ launches PID 1 (/sbin/init -> systemd).                │
└────────────────────────────────────────────────────────┘
```

---

## 4. Bootloaders: GRUB and Windows Boot Manager

- **Bootloader Role:** The operating system kernel is stored as a passive compressed file on disk (`/boot/vmlinuz-6.5.0`). The CPU hardware cannot execute a compressed file directly from an ext4 filesystem without a translator. The bootloader bridges firmware to the OS kernel.
- **GRUB 2 (GRand Unified Bootloader):** The standard Linux bootloader.
  - Features modular filesystem drivers capable of reading ext4, XFS, Btrfs, and ZFS.
  - Presents the interactive boot menu, allows editing kernel command-line parameters (`nomodeset`, `single`, `systemd.unit=rescue.target`), and locates `initramfs`.
- **Windows Boot Manager (`bootmgr` / `winload.efi`):** The Microsoft Windows equivalent, reading the Boot Configuration Data (BCD) store to start the Windows NT kernel (`ntoskrnl.exe`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Motherboard Buses and Peripherals](./06-Motherboard-Buses-and-Peripherals.md) | [Index](../../../README.md) | [08 - Virtualization Hypervisors and Containers →](./08-Virtualization-Hypervisors-and-Containers.md) |
