# 05 - Azure Database for PostgreSQL & MySQL: Flexible Server Architecture

> **Focus:** Single Server (Retired) vs. Flexible Server, Zone-Redundant High Availability ($RPO=0$), PostgreSQL vs. MySQL decision criteria, and minimal-downtime DMS migration with CDC.

---

## 🏛️ 1. Flexible Server vs. Legacy Single Server

| Feature | Single Server (Retiring) | Flexible Server (Production Standard) |
| :--- | :---: | :---: |
| **Stop / Start Compute** | ❌ No (Billed 24/7) | ✅ **Yes** (Save 70%+ cost when idle) |
| **Zone Redundant HA** | ❌ No | ✅ **Yes** (Automatic cross-AZ failover) |
| **Burstable Compute (B-Series)**| ❌ No | ✅ **Yes** (Cost-effective for dev/test) |
| **VNet Private Injection** | Service Endpoints only | ✅ **Native VNet Subnet Injection** |

---

## 🛡️ 2. Zone-Redundant High Availability

```text
Region: East US
┌────────────────────────────────────────────────────────┐
│  Availability Zone 1          Availability Zone 3      │
│  ┌────────────────┐           ┌──────────────────┐     │
│  │ PRIMARY SERVER │ ──sync──► │  STANDBY SERVER  │     │
│  │  (Read/Write)  │           │   (Hot Standby)  │     │
│  └────────────────┘           └──────────────────┘     │
└───────────────────────────┬────────────────────────────┘
                            │
            Single Resilient DNS Endpoint
          (Auto-failover in 60 to 120 seconds, RPO = 0)
```

- **Synchronous Replication:** Transactions commit on the primary only after written to standby WAL storage, ensuring **zero data loss ($RPO = 0$)**.
- **Automatic Health Failover:** Standby server is automatically promoted in 60–120 seconds ($RTO$) without application connection string reconfiguration.

---

### 💻 Production Azure CLI Flexible Server Deployment

```bash
az postgres flexible-server create   --name contoso-postgres-001   --resource-group contoso-rg   --location eastus   --admin-user pgadmin   --admin-password "P@ssword1234!"   --sku-name Standard_D2s_v3   --tier GeneralPurpose   --storage-size 128   --version 16   --high-availability ZoneRedundant   --zone 1   --standby-zone 3
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Cosmos DB Consistency Levels](./04-Cosmos-DB-Consistency-Levels.md) | [Index](../../../README.md) | [06 - Azure Cache for Redis and Database Caching →](./06-Azure-Cache-for-Redis-and-Database-Caching.md) |
