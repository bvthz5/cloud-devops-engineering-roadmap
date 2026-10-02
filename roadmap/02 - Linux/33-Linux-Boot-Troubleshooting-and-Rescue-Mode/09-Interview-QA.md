# 09 — Linux Boot Troubleshooting Interview Q&A

10 technical interview questions for SRE, Systems Administrator, and Cloud Engineer roles.

---

### Q1: What is the purpose of the initial ramdisk (initramfs / initrd)?
**Answer:**
The initramfs is a temporary, minimal root filesystem loaded into memory by the bootloader along with the kernel. Its purpose is to provide the kernel with essential storage, filesystem, and bus drivers (e.g. NVMe, SCSI, LVM, software RAID, encrypted LUKS) required to detect, unlock, and mount the real root filesystem on the physical or cloud disk. Once the real root is mounted onto `/sysroot`, the kernel executes `pivot_root` (or `switch_root`) and discards the initramfs from memory.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
