# 1. Storage Fundamentals: Disks, Partitions, Filesystems & Mounts

## Storage Layer Hierarchy

Understanding Linux storage requires visualizing the layer stack from physical hardware to user applications:

```text
[ User Applications / Programs ]
              ↓
[ Virtual File System (VFS) Abstraction ]
              ↓
[ Filesystem (ext4, XFS, Btrfs) ]
              ↓
[ Partition / Logical Volume (sda1, vg0-lv_data) ]
              ↓
[ Partition Table (GPT / MBR) ]
              ↓
[ Physical Disk Device (/dev/sda, /dev/nvme0n1) ]
```

## Key Definitions

1. **Storage Device (`/dev/sdX` or `/dev/nvmeXn1`):** Raw physical disk attached to the machine (SATA, NVMe, SCSI, USB).
2. **Partition Table:** Header at the beginning of the disk defining how disk space is divided into logical slices.
3. **Partition:** A contiguous boundary slice of raw disk space (e.g., `/dev/sda1`).
4. **Filesystem:** Data structure format written onto a partition that allows the OS to organize, store, retrieve, and control files and metadata.
5. **Mounting & Mount Point:** Attaching a formatted filesystem to a specific directory path in the unified Linux root directory tree (`/`).
