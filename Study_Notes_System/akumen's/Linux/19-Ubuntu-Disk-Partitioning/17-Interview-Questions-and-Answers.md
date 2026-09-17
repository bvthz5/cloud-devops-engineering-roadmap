# 17. Storage & Partitioning Interview Q&A

1. **What is the difference between MBR and GPT?**
   MBR supports max 2TB disks and 4 primary partitions. GPT supports up to 9.4ZB disks, 128 partitions, and includes backup redundant headers and CRC32 checksums.

2. **Why is mounting by UUID preferable to `/dev/sda1` in `/etc/fstab`?**
   Kernel device node names (`/dev/sda`, `/dev/sdb`) can change across reboots depending on hardware discovery order. UUIDs remain static.

3. **What is the purpose of the EFI System Partition (ESP)?**
   A FAT32 partition storing UEFI bootloaders (`grubx64.efi`), kernel images, and drivers required by UEFI firmware to boot the system.

4. **How do you extend an active `ext4` filesystem on LVM?**
   Run `sudo lvextend -L +10G /dev/vg/lv` followed by `sudo resize2fs /dev/vg/lv`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - Troubleshooting Partitioning Scenarios](./16-Troubleshooting-Partitioning-Scenarios.md) | [README](./README.md) | [18 - MCQs and Quick Revision](./18-MCQs-and-Quick-Revision.md) |
