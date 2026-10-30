# 03 - Amazon Aurora: Distributed Cloud-Native Storage & Aurora Serverless v2

> **Focus:** Cloud-native distributed relational database, 6-way storage replication across 3 AZs, up to 15 read replicas with sub-10ms lag, Aurora Serverless v2 ACU auto-scaling, and Aurora Global Database.

---

## 🏛️ 1. Amazon Aurora Distributed Storage Architecture

Amazon Aurora is AWS's proprietary cloud-native relational database engine compatible with **PostgreSQL** and **MySQL**. It separates compute from storage:

```text
┌────────────────────────────────────────────────────────┐
│                   Compute Instances                    │
│   [Primary Writer] ◄──────────────► [Read Replica 1]   │
└───────────────────────────┬────────────────────────────┘
                            │ Distributed Storage Protocol
                            ▼
┌────────────────────────────────────────────────────────┐
│             Shared Aurora Cluster Volume               │
│                                                        │
│  AZ 1a: [Copy 1] [Copy 2]                              │
│  AZ 1b: [Copy 3] [Copy 4]  (6 copies across 3 AZs)    │
│  AZ 1c: [Copy 5] [Copy 6]                              │
│                                                        │
│  • Auto-scales disk storage up to 128 TB               │
│  • Self-healing: Continually repairs bad sectors       │
│  • Quorum write model (4 of 6 copies must confirm)     │
└────────────────────────────────────────────────────────┘
```

---

## ⚡ 2. Aurora Serverless v2 & Aurora Global Database

- **Aurora Serverless v2:** Scales database compute capacity instantly in fractions of a second based on load using **Aurora Capacity Units (ACUs)** (1 ACU = 2 GB RAM). Scales smoothly without dropping client connections.
- **Aurora Global Database:** Replicates data across up to 6 AWS regions with typical latency of **less than 1 second**. Delivers cross-region disaster recovery with $RTO < 1	ext{ minute}$.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - RDS High Availability Multi AZ and Read Replicas](./02-RDS-High-Availability-Multi-AZ-and-Read-Replicas.md) | [Index](../../../README.md) | [04 - DynamoDB NoSQL Partition Keys GSIs LSIs and Streams →](./04-DynamoDB-NoSQL-Partition-Keys-GSIs-LSIs-and-Streams.md) |
