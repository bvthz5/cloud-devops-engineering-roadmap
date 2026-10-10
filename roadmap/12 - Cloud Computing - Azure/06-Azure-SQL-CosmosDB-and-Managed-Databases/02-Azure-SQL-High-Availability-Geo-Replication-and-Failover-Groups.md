# 02 - Azure SQL High Availability, Active Geo-Replication & Failover Groups

> **Focus:** Zero-data-loss architecture, Active Geo-Replication readable secondaries, Auto-Failover Groups, Long-Term Retention (LTR up to 10 years), and DTU resource contention troubleshooting.

---

## 🌍 1. Active Geo-Replication vs. Auto-Failover Groups

```text
Primary Database (East US)
      │
      ├─── Async Replication ───► Secondary Replica 1 (West US)  [READ-ONLY POOL]
      └─── Async Replication ───► Secondary Replica 2 (Europe)   [READ-ONLY POOL]
```

### 📊 Comparison Matrix:

| Feature | Active Geo-Replication | Auto-Failover Groups |
| :--- | :--- | :--- |
| **Number of Secondaries** | Up to **4 readable secondaries** in any region. | Exactly **1 secondary** in a paired region. |
| **Failover Mechanism** | **Manual only** (User must trigger failover). | **Automatic** or Manual (Health agent driven). |
| **Connection Endpoint**| Separate connection string per server. | **Single DNS Listener Endpoint** (Auto-redirects). |
| **RPO & RTO** | RPO < 5 seconds, RTO < 30 minutes | **RPO < 5 seconds, RTO < 60 seconds** |

---

## 💾 2. Backups & Long-Term Retention (LTR)

Azure SQL automatically captures continuous transaction log backups (every 5–10 mins) and full weekly backups:
- **Short-Term Retention:** 7 to 35 days point-in-time restore (PITR).
- **Long-Term Retention (LTR):** Store full database backups in Azure Blob Storage with RA-GRS redundancy for up to **10 years** for regulatory audit compliance.

```bash
# Configure 10-Year Long-Term Retention (LTR) Policy
az sql db ltr-policy set   --resource-group contoso-rg   --server contoso-sqlserver-001   --database contoso-db   --weekly-retention P4W   --monthly-retention P12M   --yearly-retention P10Y   --week-of-year 1
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure SQL Database Single Elastic Pool Managed Instance](./01-Azure-SQL-Database-Single-Elastic-Pool-Managed-Instance.md) | [Index](../../../README.md) | [03 - Azure Cosmos DB Multi Model NoSQL Architecture →](./03-Azure-Cosmos-DB-Multi-Model-NoSQL-Architecture.md) |
