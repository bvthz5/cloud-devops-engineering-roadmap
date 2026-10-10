# 06 - Azure Cache for Redis, Caching Architecture & Disaster Recovery Design

> **Focus:** Sub-millisecond in-memory caching, Redis Tiers (Basic, Standard, Premium, Enterprise), the Cache-Aside pattern, low cache hit rate diagnosis, LRU memory eviction, and multi-region Disaster Recovery architecture.

---

## ⚡ 1. Azure Cache for Redis Tiers

| Tier | Memory Size | Clustering | Data Persistence | VNet Private Injection | Best Enterprise Use Case |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Basic** | 250 MB – 53 GB | ❌ No | ❌ No | ❌ No | Dev/test only (No SLA, single node). |
| **Standard**| 250 MB – 53 GB | ❌ No | ❌ No | ❌ No | Production with high availability (Two-node primary/replica). |
| **Premium** | 6 GB – 1.2 TB | ✅ Up to 10 Shards | ✅ **RDB & AOF** | ✅ **Yes** | **Mission-critical banking & retail applications**. |
| **Enterprise**| 12 GB+ | ✅ Up to 30 Shards | ✅ Yes | ✅ Yes | Active-active geo-replication, RedisJSON, RediSearch. |

---

## 🔄 2. The Cache-Aside (Lazy Loading) Pattern

```text
Application receives Read Request
             │
             ▼
      ┌───────────────┐   Cache HIT
      │  Redis Cache  │ ─────────────► Return cached JSON immediately!
      └──────┬────────┘
             │ Cache MISS
             ▼
      ┌───────────────┐
      │ Azure SQL DB  │ ─────────────► Fetch data from SQL
      └──────┬────────┘
             │
             ▼
      Write result to Redis with TTL (e.g. 600s)
             │
             ▼
      Return response to user
```

```python
import redis
import json

r = redis.StrictRedis(
    host="contoso-redis-001.redis.cache.windows.net",
    port=6380,
    password="<redis-primary-key>",
    ssl=True
)

def get_product(product_id):
    cache_key = f"product:{product_id}"
    cached = r.get(cache_key)
    if cached:
        return json.loads(cached)  # Cache HIT
    
    # Cache MISS - Query SQL Database
    product = db.query(f"SELECT * FROM products WHERE id = {product_id}")
    r.setex(cache_key, 600, json.dumps(product))  # Cache with 10 min TTL
    return product
```

---

## 🩺 3. Diagnosing a Low Cache Hit Rate (< 50%)

1. **TTL Too Short:** Keys expire before subsequent requests arrive. Increase TTL.
2. **Missing Cache Invalidation:** Key naming inconsistency (`product:1` vs. `item:1`). Standardize cache keys.
3. **Improper Eviction Policy:** If Redis hits max memory, configure LRU (Least Recently Used) eviction:
   ```bash
   az redis update      --name contoso-redis-001      --resource-group contoso-rg      --set "redisConfiguration.maxmemory-policy=allkeys-lru"
   ```

---

## 🏛️ 4. Enterprise Multi-Region Disaster Recovery (DR) Architecture

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        PRIMARY REGION (East US)                        │
│                                                                        │
│  App Service  ──► Azure SQL DB (BC Tier) ──► Cosmos DB  ──► Redis      │
│  (Active)         (Primary Server)          (Multi-Write)   (Primary)  │
└───────────────────────┬───────────────────────────┬────────────┬───────┘
                        │ Sync Replication          │ Multi-Sync │ Geo-Replica
                        ▼                           ▼            ▼
┌────────────────────────────────────────────────────────────────────────┐
│                        SECONDARY REGION (West US)                      │
│                                                                        │
│  App Service  ──► SQL Failover Replica  ──► Cosmos DB  ──► Redis      │
│  (Standby)        (Auto-Promoted)           (Active)        (Passive)  │
└────────────────────────────────────────────────────────────────────────┘
                                    ▲
                          Azure Traffic Manager
                     (Global DNS Health Probe Routing)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Azure Database for PostgreSQL and MySQL Flexible Server](./05-Azure-Database-for-PostgreSQL-and-MySQL-Flexible-Server.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
