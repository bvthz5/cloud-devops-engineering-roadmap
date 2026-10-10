# 04 - Cosmos DB Consistency Levels & Troubleshooting HTTP 429 Errors

> **Focus:** The 5 consistency levels (Strong, Bounded Staleness, Session, Consistent Prefix, Eventual), PACELC theorem trade-offs, and production resolution of HTTP 429 Too Many Requests errors.

---

## ⚖️ 1. The 5 Consistency Levels

Cosmos DB replaces the binary choice between strict ACID consistency and eventual consistency with 5 precisely defined levels:

```text
STRONG ◄──────── BOUNDED STALENESS ──────── SESSION ──────── CONSISTENT PREFIX ────────► EVENTUAL
(Highest Latency,                               (Default,                               (Lowest Latency,
 Strongest Guarantees)                           Most Popular)                           Highest Throughput)
```

| Consistency Level | Guarantee | Typical Enterprise Use Case |
| :--- | :--- | :--- |
| **Strong** | Linearizability. Reads are guaranteed to return the latest committed write globally. Highest latency. | Financial transactions, inventory reservation. |
| **Bounded Staleness** | Reads lag behind writes by at most *K* versions or *T* time intervals. | Global retail catalogs requiring near-real-time synchronization. |
| **Session (Default)** | Monotonic reads and writes within a user's session token. | User profile updates, e-commerce shopping carts, social posts. |
| **Consistent Prefix** | Reads never observe out-of-order writes; updates appear in proper sequence. | Social media feeds, event streams. |
| **Eventual** | No ordering guarantee. Given enough time without updates, all replicas converge. | YouTube view counts, social media likes, telemetry dashboards. |

---

## 🚨 2. Troubleshooting HTTP 429 Errors (Too Many Requests)

An **HTTP 429** response signifies that the application's request rate has exceeded the provisioned RU/s capacity:

```bash
# 1. Immediate Incident Mitigation: Scale Up Throughput Temporarily
az cosmosdb sql container throughput update   --account-name contoso-cosmos-001   --resource-group contoso-rg   --database-name contoso-nosqldb   --name orders   --throughput 10000

# 2. Permanent Architectural Fix: Migrate from Fixed Throughput to Autoscale
az cosmosdb sql container throughput migrate   --account-name contoso-cosmos-001   --resource-group contoso-rg   --database-name contoso-nosqldb   --name orders   --throughput-type autoscale
```

### 🛠️ Application-Level Remediation Checklist:
1. **SDK Exponential Backoff:** Verify that Cosmos DB SDK automatic retry policy with exponential backoff is active.
2. **Cache Read Queries in Redis:** Offload repetitive catalog reads to Azure Cache for Redis.
3. **Partition-Scoped Queries:** Ensure queries include the partition key to eliminate high-RU cross-partition fan-out queries.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Azure Cosmos DB Multi Model NoSQL Architecture](./03-Azure-Cosmos-DB-Multi-Model-NoSQL-Architecture.md) | [Index](../../../README.md) | [05 - Azure Database for PostgreSQL and MySQL Flexible Server →](./05-Azure-Database-for-PostgreSQL-and-MySQL-Flexible-Server.md) |
