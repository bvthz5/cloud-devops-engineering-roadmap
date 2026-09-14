# 5. Manual Installation & Dual-Booting Guidelines

## Dual-Booting Ubuntu alongside Windows

### Step 1: Windows Pre-Requisites
1. Open Windows **Disk Management** (`diskmgmt.msc`) and shrink Windows `C:` partition to create **Unallocated Space**.
2. Disable Windows **Fast Startup** in Power Options (prevents Windows from locking NTFS partitions in hibernated state).
3. Disable BitLocker encryption or record BitLocker recovery keys before installing Ubuntu.

### Step 2: Ubuntu Installer ("Something Else" Option)
1. Select unallocated space.
2. Ensure UEFI ESP partition exists (reuse existing Windows EFI partition at `/boot/efi` without formatting it).
3. Create `/` (Root) partition formatted as `ext4`.
4. Install GRUB bootloader to the primary disk device (e.g., `/dev/nvme0n1`).
