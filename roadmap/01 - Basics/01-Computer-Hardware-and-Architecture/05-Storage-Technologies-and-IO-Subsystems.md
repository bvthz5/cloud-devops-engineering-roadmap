# 05 - Storage Technologies & Storage I/O Subsystems

---

## 1. Storage Fundamentals: Volatile vs. Non-Volatile

While registers, caches, and DRAM are volatile (losing their entire state when electrical power ceases), persistent storage provides non-volatile retention for operating system kernels, databases, container images, and log files.

In DevOps, storage architecture dictates database write latency, backup throughput, and container startup times.

---

## 2. Hard Disk Drives (HDD): Mechanical Magnetic Storage

Hard Disk Drives store data magnetically on rapidly spinning aluminum or glass platters (typically 5,400, 7,200, or 10,000 RPM).

```text
       Spindle Motor (Rotates Platters at 7200 RPM)
            │
      ┌─────▼─────┐
      │  Platter  │  <── Track (Concentric rings)
      │    ┌───┐  │  <── Sector (512B or 4KB atomic unit)
      │    └───┘  │
      └───────────┘
            ▲
            │ Actuator Arm moves Read/Write Head radially
```

### The Three Sources of HDD Latency:
1. **Seek Time:** Time required for the physical actuator arm to move across tracks (3–10 ms).
2. **Rotational Latency:** Time waiting for the rotating sector to pass underneath the head (~4.16 ms at 7200 RPM).
3. **Transfer Time:** Time to read the bits magnetically into the drive buffer.

> **DevOps Bottom Line:** HDDs are physically incapable of delivering high random IOPS (rarely exceeding 75–150 IOPS). They remain viable only for bulk archival storage, cold backup targets, and S3 Glacier equivalents.

---

## 3. Solid State Drives (SSD): NAND Flash Architecture

Solid State Drives replace spinning mechanical parts with non-volatile **NAND Flash Memory**.
- **Silicon Structure:** Organized into **Pages** (typically 4 KB or 8 KB) grouped into **Blocks** (typically 128 to 512 Pages, 1 MB to 4 MB).
- **The Asymmetric Erase Rule:**
  - Reads and Writes occur at the **Page** level.
  - Erases can ONLY occur at the **Block** level!
- **Write Amplification & Flash Translation Layer (FTL):** When modifying existing data, the SSD must copy unchanged pages to an empty block, erase the old block, and write the new data. The onboard microcontroller runs the FTL to manage Wear Leveling, Garbage Collection, and TRIM commands.

---

## 4. SATA SSD vs. NVMe SSD (The Bus Revolution)

The greatest bottleneck of early SSDs was not the flash memory itself, but the legacy **SATA interface and AHCI software protocol**, which were designed in the 1990s for slow mechanical disks.

```text
Legacy SATA Architecture:
NAND Flash ──> SATA Controller ──> AHCI Protocol ──> SATA Cable ──> Southbridge ──> CPU
(Bottlenecked by AHCI single queue with 32 command depth and SATA 6 Gbps bandwidth: ~550 MB/s)

Modern NVMe Architecture:
NAND Flash ──> NVMe Controller ════════════ PCIe Direct Bus ════════════> CPU
(Direct memory bus access, 64,000 parallel queues with 64,000 commands each: Up to 14,000+ MB/s)
```

### Comprehensive Comparison: SATA vs. NVMe

| Metric / Dimension | SATA III SSD (AHCI) | NVMe SSD (PCIe Gen 4 / Gen 5) |
|---|---|---|
| **Underlying Protocol** | AHCI (Legacy 1990s disk protocol) | NVMe (Built from scratch for parallel flash) |
| **Physical Interface** | SATA 6 Gbps bus or M.2 SATA | PCIe Gen 4 x4 / Gen 5 x4 lanes |
| **Maximum Sequential Read** | ~550 MB/s | **7,000 to 14,500 MB/s** |
| **Random Read/Write IOPS** | Max ~90,000 IOPS | **1,000,000 to 3,000,000+ IOPS** |
| **Command Queues** | 1 Single Queue | **64,000 Parallel Queues** |
| **Command Queue Depth** | 32 commands maximum | **64,000 commands per queue** |
| **CPU Overhead** | High (multiple register reads per I/O) | Extremely low (single doorbell register write) |
| **Cloud Counterparts** | AWS `gp2` / standard EBS | AWS `io2 Block Express`, `i3en/i4i` local NVMe |

---

## 5. Storage I/O Metrics: Throughput, IOPS & Latency

DevOps engineers configure, provision, and troubleshoot storage based on three interdependent metrics:

$$\text{Throughput (Bytes/sec)} = \text{IOPS} \times \text{I/O Block Size (Bytes)}$$

1. **IOPS (Input/Output Operations Per Second):** Number of distinct read or write transactions executed per second. Critical for transactional databases (OLTP: PostgreSQL, MongoDB, etcd in Kubernetes).
2. **Throughput (MB/s):** The raw volume of data transferred per second. Critical for batch streaming, backups, log dumps, and video encoding (OLAP).
3. **I/O Latency:** The round-trip time required for an I/O request to be completed by the storage device. High latency causes CPU threads to enter the uninterruptible sleep state (`D` state in Linux), generating high **`iowait`** CPU load.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Virtual Memory Paging and MMU](./04-Virtual-Memory-Paging-and-MMU.md) | [README](./README.md) | [06 - Motherboard Buses and Peripherals](./06-Motherboard-Buses-and-Peripherals.md) |
