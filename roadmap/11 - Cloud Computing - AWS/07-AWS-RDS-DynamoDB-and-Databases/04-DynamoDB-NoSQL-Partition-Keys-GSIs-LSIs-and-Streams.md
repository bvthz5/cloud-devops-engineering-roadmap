# 04 - Amazon DynamoDB: NoSQL Architecture, Partition Keys, GSIs & Streams

> **Focus:** Fully managed NoSQL key-value and document store, single-digit millisecond latency, Partition Key vs. Sort Key, Capacity calculations (RCU/WCU), Secondary Indexes (GSI vs. LSI), and DynamoDB Streams (CDC).

---

## ⚡ 1. Amazon DynamoDB Architecture

**Amazon DynamoDB** is a fully managed, serverless NoSQL database that delivers single-digit millisecond response times at any scale with unlimited storage and throughput capacity:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   DynamoDB Table: "OrderTransactions"                  │
│                                                                        │
│   PRIMARY KEY (Composite):                                             │
│   ├── Partition Key (PK / Hash Key): "CustomerID" (Distributes data)   │
│   └── Sort Key (SK / Range Key):     "OrderTimestamp" (Sorts items)    │
│                                                                        │
│   Attributes: Items contain flexible schema JSON-like key-value pairs  │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
       ┌────────────────────────────┼───────────────────────────┐
       ▼                            ▼                           ▼
┌──────────────┐             ┌──────────────┐            ┌──────────────┐
│ Partition 1  │             │ Partition 2  │            │ Partition 3  │
│ Customer: C1 │             │ Customer: C2 │            │ Customer: C3 │
└──────────────┘             └──────────────┘            └──────────────┘
```

---

## 🧮 2. Read & Write Capacity Units (RCU & WCU) Calculation

- **1 Write Capacity Unit (WCU):** 1 write per second for an item up to **`1 KB`** in size.
  - Formula: $	ext{WCU} = lceil	ext{Item Size in KB}ceil 	imes 	ext{Writes per second}$.
- **1 Read Capacity Unit (RCU):**
  - **Strongly Consistent Read:** 1 read per second for an item up to **`4 KB`** in size.
  - **Eventually Consistent Read (Default):** 2 reads per second for an item up to **`4 KB`** in size (50% cheaper!).

---

## 🔍 3. Global Secondary Indexes (GSIs) vs. Local Secondary Indexes (LSIs)

| Feature | Global Secondary Index (GSI) | Local Secondary Index (LSI) |
| :--- | :--- | :--- |
| **Partition Key** | Can be **different** from table partition key. | **Must be identical** to table partition key. |
| **Sort Key** | Any attribute or optional. | Different attribute from table sort key. |
| **Creation Time** | Created **anytime** (during or after table creation). | **Only at table creation time!** |
| **Capacity Allocation**| Has its own independent provisioned RCU/WCU. | Uses the parent table's RCU/WCU pool. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Amazon Aurora Serverless Global Databases and Storage](./03-Amazon-Aurora-Serverless-Global-Databases-and-Storage.md) | [Index](../../../README.md) | [05 - ElastiCache Redis Memcached and Database Caching →](./05-ElastiCache-Redis-Memcached-and-Database-Caching.md) |
