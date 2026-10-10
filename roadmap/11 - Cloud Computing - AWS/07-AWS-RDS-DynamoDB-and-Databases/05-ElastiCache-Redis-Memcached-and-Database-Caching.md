# 05 - Amazon ElastiCache: Redis vs. Memcached & Caching Strategies

> **Focus:** In-memory caching architecture, Redis vs. Memcached comparison, Cache-Aside vs. Write-Through patterns, and sub-millisecond database offloading.

---

## ⚡ 1. ElastiCache Redis vs. Memcached

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│        Amazon ElastiCache Redis      │       │     Amazon ElastiCache Memcached     │
│                                      │       │                                      │
│  • Complex data structures (Sets,    │       │  • Pure simple key-value store       │
│    Hashes, Lists, Bitmaps, Streams)  │       │  • Multi-threaded memory engine      │
│  • In-memory persistence (AOF/RDB)   │       │  • No persistence (Purely ephemeral) │
│  • Multi-AZ with auto-failover       │       │  • Simple horizontal node sharding   │
│  • Pub/Sub messaging capabilities    │       │  • No replication or multi-AZ        │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

---

## 🔄 2. Production Caching Strategies

1. **Cache-Aside (Lazy Loading):** Application queries the cache first. On a cache hit, returns data. On a cache miss, queries database, writes result to cache with a TTL, and returns data.
2. **Write-Through:** Application writes to cache and database simultaneously. Guarantees cache is never stale, but incurs higher write latency.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - DynamoDB NoSQL Partition Keys GSIs LSIs and Streams](./04-DynamoDB-NoSQL-Partition-Keys-GSIs-LSIs-and-Streams.md) | [Index](../../../README.md) | [06 - AWS Database Migration Service DMS and Schema Conversion Tool →](./06-AWS-Database-Migration-Service-DMS-and-Schema-Conversion-Tool.md) |
