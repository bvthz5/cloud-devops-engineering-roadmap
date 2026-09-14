# 2. MBR vs GPT Partition Tables

| Feature | MBR (Master Boot Record) | GPT (GUID Partition Table) |
| --- | --- | --- |
| **Max Disk Capacity** | 2.2 Terabytes (2 TB) | 9.4 Zettabytes (9.4 Billion TB) |
| **Primary Partition Limit** | 4 Primary (or 3 Primary + 1 Extended) | 128 Primary partitions (Linux default) |
| **Backup Partition Table** | No (Single sector at Sector 0) | Yes (Header duplicated at end of disk) |
| **Integrity Verification** | None | CRC32 Checksums for corruption detection |
| **Boot Architecture** | Legacy BIOS | UEFI (Unified Extensible Firmware Interface) |
| **Standard Recommendation** | Legacy embedded systems only | Modern Standard for all systems |

## MBR Architecture Details
- Occupies the first 512 bytes of the disk (Sector 0).
- Contains 446 bytes of Bootstrap code + 64 bytes Partition Table (16 bytes × 4 partitions) + 2 bytes boot signature (`0x55AA`).

## GPT Architecture Details
- Uses Logical Block Addressing (LBA).
- LBA 0: Protective MBR (prevents legacy disk tools from overwriting GPT).
- LBA 1: Primary GPT Header.
- LBA 2-33: Partition Entries (128 partition slots).
- Secondary GPT Header stored at the absolute last sectors of the disk for emergency recovery.
