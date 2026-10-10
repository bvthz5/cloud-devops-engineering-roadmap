# 02 - Azure Managed Disks: Ultra, Premium SSD v1/v2, Standard SSD & HDD

> **Focus:** Block storage performance tiers, IOPS and throughput ratings, host caching policies (None, Read-Only, Read/Write), disk snapshots, encryption standards (SSE, ADE), and zone-redundant disks.

---

## 💽 1. Azure Managed Disks Comparison Matrix

Azure Managed Disks are virtual block storage devices attached directly to Azure VMs:

| Disk Type | Underlying Media | Max IOPS | Max Throughput | Latency | Typical Enterprise Workload |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Standard HDD** | Magnetic Spindle | `2,000` | `500 MB/s` | High (`>10 ms`) | Dev/test machines, infrequent access, batch backups. Cheapest option. |
| **Standard SSD** | Solid State | `6,000` | `750 MB/s` | Low (`<10 ms`) | Web servers, lightly loaded business applications, staging environments. |
| **Premium SSD (v1)**| Solid State | `20,000` | `900 MB/s` | Low (`<5 ms`) | Production enterprise databases, high-transaction workloads, SAP. |
| **Premium SSD v2** | Next-Gen SSD | `80,000` | `1,200 MB/s` | Sub-millisecond | Mission-critical SQL Server, PostgreSQL, Cassandra, dynamic IOPS scaling. |
| **Ultra Disk** | NVMe PCIe Flash | `400,000` | `4,000 MB/s` | **Sub-millisecond** | Extreme transaction processing, SAP HANA, top-tier financial analytics. |

---

## ⚡ 2. Host Caching Options & Best Practices

Host caching leverages the physical hypervisor server's memory and high-speed local NVMe to accelerate read and write operations:

| Caching Mode | Behavior | Recommended Workload |
| :--- | :--- | :--- |
| **None** | Bypasses host cache completely. All I/O hits persistent Azure storage directly. | **Write-heavy workloads** (Database transaction logs, SQL Server `.ldf` files). |
| **Read-Only** | Reads served from local host cache. Writes bypass cache to disk. | **Read-heavy workloads** (Static assets, web content, SQL Server `.mdf` data files). |
| **Read/Write** | Reads and writes cached locally; writes written back to disk asynchronously. | Balanced OS boot disks and general file storage. |

> ⚠️ **Exam Note:** **Ultra Disk** and **Premium SSD v2** do **NOT** support host caching because their baseline latency is already sub-millisecond.

---

## 🔒 3. Managed Disk Security & Encryption Architecture

1. **Server-Side Encryption (SSE):** Enabled **by default** for all Managed Disks. Encrypts data at rest using 256-bit AES encryption with Azure platform-managed keys (PMK) or customer-managed keys (CMK) stored in Azure Key Vault.
2. **Azure Disk Encryption (ADE):** Uses Windows BitLocker or Linux DM-Crypt to encrypt both OS and data disks inside the virtual machine. Keys and secrets are safeguarded in Azure Key Vault.
3. **Encryption at Host:** Extends encryption to the temporary disk and ephemeral host cache directly on the hypervisor hardware before data traverses Azure storage network switches.

---

### 💻 Production Azure CLI Disk Commands

```bash
# 1. Create a Premium SSD Managed Disk (32 GB)
az disk create   --resource-group demo-rg   --name myDataDisk   --size-gb 32   --sku Premium_LRS

# 2. Attach Data Disk to an existing VM
az vm disk attach   --resource-group demo-rg   --vm-name my-ubuntu-vm   --name myDataDisk

# 3. Create a Point-in-Time Disk Snapshot
az snapshot create   --resource-group demo-rg   --name myDataDiskSnapshot   --source myDataDisk

# 4. Restore a New Managed Disk from Snapshot
az disk create   --resource-group demo-rg   --name myRestoredDisk   --source myDataDiskSnapshot   --sku Premium_LRS
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure Virtual Machine Series and Sizing](./01-Azure-Virtual-Machine-Series-and-Sizing.md) | [Index](../../../README.md) | [03 - High Availability Availability Sets vs Availability Zones →](./03-High-Availability-Availability-Sets-vs-Availability-Zones.md) |
