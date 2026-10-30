# 01 - Azure Storage Account Types, Replication, Durability & ADLS Gen2

> **Focus:** Storage account classifications (GPv2, Premium, Legacy), 6 Replication strategies (LRS, ZRS, GRS, GZRS, RA-GRS, RA-GZRS), Durability vs. Availability, Account Failover, and Azure Data Lake Storage Gen2 (ADLS Gen2) Hierarchical Namespace (HNS).

---

## 📦 1. Storage Account Types

A **Storage Account** is the top-level namespace container for Azure core storage primitives (Blobs, Files, Queues, Tables, and Data Lake filesystems).

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        Storage Account Types                           │
│                                                                        │
│   ┌───────────────────────┐   ┌─────────────────┐   ┌───────────────┐  │
│   │  General Purpose v2   │   │  Blob Storage   │   │    Premium    │  │
│   │       (GPv2)          │   │    (Legacy)     │   │ (Block, File, │  │
│   │                       │   │                 │   │     Page)     │  │
│   │  ✅ Industry Standard  │   │  Migrate to     │   │ SSD Sub-ms    │  │
│   │     for All Workloads │   │  GPv2           │   │ Single-Region │  │
│   └───────────────────────┘   └─────────────────┘   └───────────────┘  │
└────────────────────────────────────────────────────────────────────────┘
```

### 📊 Account Types Comparison:

| Account Type | Services Supported | Redundancy Options | Performance | Enterprise Recommendation |
| :--- | :--- | :--- | :---: | :--- |
| **General Purpose v2 (GPv2)** | Blob, Files, Queues, Tables, ADLS Gen2 | LRS, ZRS, GRS, GZRS, RA-GRS, RA-GZRS | Standard | ✅ **Default choice for all standard cloud workloads**. Supports Hot, Cool, Cold, and Archive tiers. |
| **General Purpose v1 (GPv1)** | Blob, Files, Queues, Tables | LRS, GRS, RA-GRS | Standard | Legacy. Higher transaction pricing; upgrade immediately to GPv2. |
| **Blob Storage (Legacy)** | Block & Append Blobs only | LRS, GRS, RA-GRS | Standard | Legacy. Microsoft recommends migrating all instances to GPv2. |
| **Premium Block Blobs** | Block & Append Blobs | LRS, ZRS | Premium (SSD) | High-throughput big data analytics, low-latency blob I/O. **No Archive tier.** |
| **Premium File Shares** | Azure Files only | LRS, ZRS | Premium (SSD) | Enterprise shared drives, NFS 4.1 file shares, sub-millisecond file I/O. |
| **Premium Page Blobs** | Page Blobs only | LRS | Premium (SSD) | Legacy unmanaged virtual machine disks (superseded by Managed Disks). |

> ⚠️ **Exam & Production Note:** Premium storage accounts are **single-region only** (LRS or ZRS). They do **NOT** support geo-replication (GRS) and do **NOT** support the Archive access tier.

---

## 🔄 2. Replication & Redundancy Architecture

Azure Storage guarantees extreme data durability by maintaining multiple synchronized copies of every file:

```text
Single Region Redundancy:
  • LRS (Locally Redundant Storage): 3 copies inside 1 Datacenter.
  • ZRS (Zone Redundant Storage):    3 copies across 3 physically separate Availability Zones.

Multi-Region (Geo) Redundancy:
  • GRS (Geo-Redundant Storage):      LRS (Primary Region) ──async──► LRS (Secondary Region) [6 copies total]
  • GZRS (Geo-Zone Redundant Storage): ZRS (Primary Region) ──async──► LRS (Secondary Region) [6 copies total]
  • RA-GRS / RA-GZRS:                  Same as above + Read-Only secondary endpoint is ALWAYS accessible!
```

### 🛡️ Durability vs. Availability Matrix:

| Replication | Total Copies | Physical Distribution | Read Secondary Endpoint? | SLA Availability | Annual Durability | Best Enterprise Use Case |
| :---: | :---: | :--- | :---: | :---: | :---: | :--- |
| **LRS** | 3 | 1 Datacenter, 1 Region | ❌ No | `99.9%` | **11 Nines** (`99.999999999%`) | Dev/test, non-critical data easily regenerated. |
| **ZRS** | 3 | 3 Availability Zones, 1 Region | ❌ No | `99.99%` | **12 Nines** (`99.9999999999%`) | High-availability production within a single geographic region. |
| **GRS** | 6 | 2 Paired Regions (3 DC1 + 3 DC2) | ❌ Only after failover | `99.9%` | **16 Nines** | Cross-region Disaster Recovery with low recovery time objectives. |
| **GZRS** | 6 | 3 AZs (Primary) + 1 DC (Secondary) | ❌ Only after failover | `99.99%` | **16 Nines** | Maximum resilience against zone outages AND regional catastrophes. |
| **RA-GRS** | 6 | 2 Paired Regions | ✅ **Always Readable** | `99.99%` (Read) | **16 Nines** | Active read scaling for global reporting apps + DR resilience. |
| **RA-GZRS**| 6 | 3 AZs (Primary) + 1 DC (Secondary) | ✅ **Always Readable** | `99.99%` (Read) | **16 Nines** | Mission-critical financial/healthcare systems requiring instant read failover. |

---

### 🚨 Failover Mechanics & The RA-GRS Secondary Endpoint

With **RA-GRS** or **RA-GZRS**, your storage account exposes two separate DNS endpoints:
- **Primary Endpoint:** `https://mystorageacct.blob.core.windows.net` (Read/Write)
- **Secondary Endpoint:** `https://mystorageacct-secondary.blob.core.windows.net` (Read-Only)

> ⚠️ **Post-Failover Behavior:** When a customer-managed failover is triggered (`az storage account failover`), the secondary region is promoted to primary. **The account automatically reverts to LRS in the new region.** You must explicitly re-enable GRS/GZRS replication after the primary datacenter recovers!

---

## 🌊 3. Azure Data Lake Storage Gen2 (ADLS Gen2)

ADLS Gen2 is **not a separate service**; it is Azure Blob Storage with **Hierarchical Namespace (HNS)** enabled at creation time:

```text
Flat Namespace (Standard Azure Blob Storage):
  Container: analytics
    ├── blob: "raw/orders/2024/01/order_data.csv" (Virtual path string, no real directories)
    └── To rename folder: Azure must copy 1,000,000 blobs and delete originals ➔ O(n) Time Complexity!

Hierarchical Namespace (ADLS Gen2 with HNS Enabled):
  Filesystem: analytics
    └── /raw
        └── /orders
            └── /2024
                └── /01/order_data.csv (True hierarchical directory tree)
    └── To rename folder: Azure renames a single directory metadata pointer ➔ O(1) Atomic Operation!
```

### ⚡ Why ADLS Gen2 is Essential for Data Engineering:
1. **$O(1)$ Atomic Directory Operations:** Moving or renaming folders with millions of files takes milliseconds instead of hours.
2. **POSIX Access Control Lists (ACLs):** Enables fine-grained directory-level and file-level permissions (`rwx`) for users and security groups.
3. **ABFS Native Big Data Protocol:** Spark, Databricks, Synapse, and Hadoop connect directly using `abfs://` (Azure Blob File System) or `abfss://` (SSL encrypted) rather than the slow legacy `wasb://` driver.

---

### 💻 Production Azure CLI Commands

```bash
# 1. Create GPv2 Storage Account with GZRS Redundancy
az storage account create   --name mystorageacctprod   --resource-group rg-storage   --location eastus   --sku Standard_GZRS   --kind StorageV2   --min-tls-version TLS1_2   --https-only true

# 2. Create ADLS Gen2 Storage Account (HNS Flag CANNOT be modified after creation!)
az storage account create   --name mydatalakeprod   --resource-group rg-storage   --location eastus   --sku Standard_ZRS   --kind StorageV2   --enable-hierarchical-namespace true

# 3. Check Replication Status & Secondary Region
az storage account show   --name mystorageacctprod   --resource-group rg-storage   --query "{Status:statusOfPrimary, Secondary:secondaryLocation}"   --output table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-Azure-Virtual-Machines-and-VM-Scale-Sets)](../04-Azure-Virtual-Machines-and-VM-Scale-Sets/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Azure Blob Storage Architecture and Blob Types →](./02-Azure-Blob-Storage-Architecture-and-Blob-Types.md) |
