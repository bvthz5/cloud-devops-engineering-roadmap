# 01 - Azure SQL Database Architecture: PaaS vs. IaaS, DTU vs. vCore & Serverless

> **Focus:** Managed relational database architecture, PaaS vs. SQL Server on VM, DTU purchasing model vs. vCore model, Azure Hybrid Benefit (AHB), Serverless compute auto-pause, and firewall configurations.

---

## 🏛️ 1. PaaS Azure SQL Database vs. IaaS SQL Server on VM

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│       Azure SQL Database (PaaS)      │       │      SQL Server on VM (IaaS)         │
│                                      │       │                                      │
│  • Fully managed database engine     │       │  • Full Operating System root access │
│  • Automated patching & OS updates   │       │  • Manual patching & OS maintenance  │
│  • Built-in 99.99% High Availability │       │  • You configure Always On clusters  │
│  • Automated backups & PITR          │       │  • Manual backup scripts & retention │
│  • Pay for usage & serverless pause  │       │  • Billed continuously for VM specs  │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

---

## 💳 2. Purchasing Models: DTU vs. vCore

| Feature | DTU Model (Database Transaction Unit) | vCore Model (Virtual Core) |
| :--- | :--- | :--- |
| **Concept** | Bundled, pre-calculated metric blending CPU, memory, and read/write I/O in fixed ratios. | Independent compute (vCores), memory, and storage allocation. |
| **Tiers** | Basic, Standard, Premium | **General Purpose**, **Business Critical**, **Hyperscale** |
| **Azure Hybrid Benefit**| ❌ Not supported | ✅ **Supported** (Save up to 55% using existing on-prem licenses) |
| **Max Storage Size** | 4 TB | **100 TB** (In Hyperscale tier) |
| **Serverless Compute** | ❌ Not supported | ✅ **Supported** (Auto-scales and pauses when idle) |

---

## ⏸️ 3. Serverless Compute Tier Mechanics

In the **vCore Serverless** compute tier:
- You configure a minimum and maximum vCore range (e.g., `0.5` to `4.0` vCores).
- Compute automatically scales dynamically second-by-second to match query volume.
- **Auto-Pause:** If zero connections exist for a configurable idle delay (e.g., 60 minutes), compute automatically pauses. **You only pay for underlying disk storage during paused hours.**
- **Cold Start:** The very first query after an auto-pause takes 10–30 seconds to spin up compute containers.

---

### 💻 Azure CLI SQL Server & Database Deployment

```bash
# 1. Create Logical Azure SQL Server
az sql server create   --name contoso-sqlserver-001   --resource-group contoso-rg   --location eastus   --admin-user sqladmin   --admin-password "P@ssword1234!"

# 2. Deploy Serverless SQL Database with Auto-Pause
az sql db create   --resource-group contoso-rg   --server contoso-sqlserver-001   --name contoso-db   --edition GeneralPurpose   --family Gen5   --capacity 4   --compute-model Serverless   --auto-pause-delay 60

# 3. Restrict Firewall to Corporate IP
az sql server firewall-rule create   --resource-group contoso-rg   --server contoso-sqlserver-001   --name "AllowOfficeIP"   --start-ip-address 203.0.113.5   --end-ip-address 203.0.113.5
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Azure-Blob-Storage-and-Storage-Accounts)](../05-Azure-Blob-Storage-and-Storage-Accounts/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Azure SQL High Availability Geo Replication and Failover Groups →](./02-Azure-SQL-High-Availability-Geo-Replication-and-Failover-Groups.md) |
