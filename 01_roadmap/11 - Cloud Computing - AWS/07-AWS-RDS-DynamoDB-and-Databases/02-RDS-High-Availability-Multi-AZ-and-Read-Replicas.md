# 02 - Amazon RDS High Availability: Multi-AZ vs. Read Replicas

> **Focus:** Relational database resilience, Multi-AZ synchronous standby failover, Read Replicas asynchronous scaling, and Disaster Recovery combinations.

---

## ⚖️ 1. Multi-AZ (HA) vs. Read Replicas (Scaling)

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│        RDS Multi-AZ (HA / DR)        │       │      RDS Read Replicas (Read Scale)  │
│                                      │       │                                      │
│  Primary DB (AZ 1a)                  │       │  Primary DB (AZ 1a)                  │
│       │                              │       │       │                              │
│       │ Synchronous Replication      │       │       │ Asynchronous Replication     │
│       ▼                              │       │       ▼                              │
│  Standby DB (AZ 1b) [HOT STANDBY]    │       │  Read Replica 1 (AZ 1b) [READ-ONLY]  │
│                                      │       │  Read Replica 2 (AZ 1c) [READ-ONLY]  │
│  • Automatic failover in 60-120 secs │       │  • Up to 15 Read Replicas            │
│  • Standby CANNOT be queried!        │       │  • Actively queried by apps/BI       │
│  • Zero data loss (RPO = 0)          │       │  • Can be promoted to standalone DB  │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

### 📊 Master Comparison Matrix:

| Feature | RDS Multi-AZ Standby | RDS Read Replicas |
| :--- | :--- | :--- |
| **Primary Purpose** | **High Availability & Fault Tolerance** | **Performance & Read Scalability** |
| **Replication Type** | **Synchronous** ($RPO = 0$, Zero data loss). | **Asynchronous** (Small replication lag). |
| **Can Be Queried?** | ❌ **No**. Standby is idle until failover. | ✅ **Yes**. Actively offloads reporting queries. |
| **Failover Action** | **Automatic DNS failover** in 60 to 120 seconds. | Manual promotion required. |
| **Region Scope** | Same region across Availability Zones. | Can reside in Same Region OR **Cross-Region**. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Relational Database Service RDS Architecture](./01-Relational-Database-Service-RDS-Architecture.md) | [Index](../../../README.md) | [03 - Amazon Aurora Serverless Global Databases and Storage →](./03-Amazon-Aurora-Serverless-Global-Databases-and-Storage.md) |
