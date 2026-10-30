# 03 - Azure Cosmos DB: Multi-Model NoSQL Architecture & Request Units (RU/s)

> **Focus:** Globally distributed multi-model NoSQL database, supported APIs, Request Units (RU/s) currency, Partition Key design principles, and avoiding hot partitions.

---

## 🌐 1. Azure Cosmos DB Architecture

**Azure Cosmos DB** is Microsoft's globally distributed, multi-model NoSQL database designed for modern applications requiring guaranteed single-digit millisecond latency worldwide and 99.999% availability SLAs.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                          Azure Cosmos DB                               │
├───────────────────┬───────────────────┬────────────────────────────────┤
│   NoSQL (Core)    │    MongoDB API    │         Cassandra API          │
│  JSON Documents   │ Existing Mongo App│ Wide-Column Big Data / IoT     │
├───────────────────┼───────────────────┼────────────────────────────────┤
│    Gremlin API    │     Table API     │      PostgreSQL (Citus)        │
│ Graph / Networks  │ Key-Value Storage │ Distributed Relational SQL     │
└───────────────────┴───────────────────┴────────────────────────────────┘
```

---

## ⚡ 2. Request Units (RU/s) Currency

Throughput in Cosmos DB is standardized into **Request Units (RU/s)**:
- **Baseline Metric:** Reading a `1 KB` item using its unique `id` and `partitionKey` costs exactly **`1 RU`**.
- Writing a `1 KB` document costs approximately **`5 RU`**.
- Unindexed cross-partition queries cost **`10–100+ RU`**.

---

## 🔑 3. Partition Key Strategy: The #1 Design Decision

Cosmos DB divides data into physical partitions based on the **Partition Key**. Each logical partition has a **20 GB maximum limit**:

| Criteria | Good Partition Key Choice | Bad Partition Key Choice |
| :--- | :--- | :--- |
| **Cardinality** | Thousands of unique values (e.g., `customerId`, `deviceId`). | Low cardinality (e.g., `gender`, `status`). |
| **Even Distribution** | Uniform distribution of data and traffic across all partitions. | Single value dominates (e.g., `country="US"` ➔ **Hot Partition!**). |
| **Query Filter** | Included in the `WHERE` clause of frequent queries. | Never filtered on, forcing expensive cross-partition scans. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Azure SQL High Availability Geo Replication and Failover Groups](./02-Azure-SQL-High-Availability-Geo-Replication-and-Failover-Groups.md) | [Index](../../../README.md) | [04 - Cosmos DB Consistency Levels →](./04-Cosmos-DB-Consistency-Levels.md) |
