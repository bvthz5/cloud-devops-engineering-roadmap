# 01 — Storage Architecture, Block Devices, and Partitions

In Linux, all storage hardware is exposed as **Block Devices** under `/dev/`. Understanding how the kernel interfaces with storage controllers and partitions is fundamental to systems engineering.

---

## 1. Block Device Naming Conventions

| Device Path Prefix | Interface Type | Typical Storage Technology |
| :--- | :--- | :--- |
| `/dev/nvme0n1` | NVMe over PCIe | High-speed enterprise SSDs, AWS Nitro instance store/EBS |
| `/dev/sda`, `/dev/sdb` | SATA / SAS / SCSI | Standard SSDs/HDDs, cloud hypervisor virtual disks (VirtIO) |
| `/dev/vda`, `/dev/vdb` | VirtIO (KVM/QEMU) | Linux virtual machines in OpenStack, Proxmox, DigitalOcean |
| `/dev/xvd[a-z]` | Xen Virtual Block Device | Legacy AWS EC2 Xen hypervisor instances |

---

## 2. Partitioning Schemes: GPT vs MBR

| Feature | MBR (Master Boot Record) | GPT (GUID Partition Table) |
| :--- | :--- | :--- |
| **Max Disk Capacity** | 2 Terabytes (2 TB) | 9.4 Zettabytes (9.4 ZB) |
| **Max Primary Partitions**| 4 Primary partitions (or 3 + 1 Extended) | 128 Primary partitions |
| **Redundancy** | Single partition table at sector 0 | Primary table at start + backup table at end of disk |
| **Firmware Standard** | Legacy BIOS | Modern UEFI |

---

## 3. Essential Storage Inspection Commands

```bash
# Display block device hierarchy with sizes and mount points
lsblk -f

# Display unique filesystem UUIDs (essential for /etc/fstab)
sudo blkid

# Partition a disk using modern GPT partitioning tool
sudo parted /dev/sdb --script mklabel gpt mkpart primary ext4 1MiB 100%

# Update kernel in-memory partition table without rebooting!
sudo partx -u /dev/sdb
# or
sudo partprobe /dev/sdb
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - LVM Architecture](./02-LVM-Logical-Volume-Manager-Architecture.md) |
