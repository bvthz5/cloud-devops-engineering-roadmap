# Database Reliability Engineering (DBRE) Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Columnar Analytical Query Starvation on Transactional OLTP Cluster

### 🚨 The Production Scenario
At month-end, financial analysts run massive aggregation queries (`SUM`, `AVG`, `GROUP BY`) across 200 million rows on the production PostgreSQL cluster. Transactional checkout queries spike in latency from 15ms to 8 seconds.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Analytical queries were run directly against the row-based OLTP database, saturating disk I/O, evicting transactional pages from the buffer cache, and holding table locks.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Isolate analytical workloads completely from transactional systems.
- Deploy Change Data Capture (CDC) using Debezium on Kafka to stream transaction logs in real-time to a dedicated OLAP database (ClickHouse, BigQuery, Snowflake).
- ClickHouse stores data columnarly, executing aggregations over millions of rows in milliseconds with zero impact on OLTP Postgres.
- If immediate offload is required, route analytical queries to a dedicated read replica with strict `statement_timeout = '15s'`.
- Enforce query governor limits to kill long-running analytical queries automatically.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Identify rogue analytical queries running on OLTP database
SELECT pid, now() - query_start AS duration, query FROM pg_stat_activity WHERE query ~* 'GROUP BY' AND state != 'idle';

# Kill blocking analytical query to restore OLTP latency
SELECT pg_terminate_backend(<ANALYTIC_QUERY_PID>);

# Enforce strict statement timeout on reporting user role
ALTER ROLE bi_analyst SET statement_timeout = '30s';

# Execute fast columnar aggregation in ClickHouse
clickhouse-client --query 'SELECT count(*), sum(amount) FROM orders WHERE created_at > now() - interval 30 day;'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Row-based OLTP databases are optimized for low-latency point lookups; running heavy columnar aggregations on them poisons the buffer pool and destroys transaction performance. I decouple OLTP and OLAP architectures using Debezium CDC to stream change streams into ClickHouse or BigQuery. Analysts get sub-second query performance and OLTP production remains unaffected."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: PostgreSQL Connection Pool Exhaustion & 'Too Many Clients Already' Outage** | `SELECT count(*), state FROM pg_stat_activity GROUP BY state;` | Microservices opened direct TCP connections to PostgreSQL without pooling (e.g.,... |
| **Scenario 2: PostgreSQL Transaction ID (TXID) Wraparound Catastrophic Read-Only Lockout** | `SELECT datname, age(datfrozenxid) FROM pg_database ORDER BY age(datfrozenxid) DESC;` | PostgreSQL uses 32-bit transaction IDs. If autovacuum fails to freeze old transa... |
| **Scenario 3: MySQL Replication Lag Spikes by Hours Due to Long Single-Threaded DDL** | `SHOW REPLICA STATUS\G` | MySQL replica SQL thread was executing in single-threaded mode (or multi-threade... |
| **Scenario 4: Redis Cluster OOM and Key Eviction Storm on Unset TTL Data** | `redis-cli --bigkeys` | Redis `maxmemory-policy` was set to `allkeys-lru` or `allkeys-lfu`. A new featur... |
| **Scenario 5: Database Deadlocks Paralyze E-Commerce Inventory Checkout** | `SELECT * FROM pg_stat_database WHERE datname = 'appdb';` | Two concurrent transactions locked the same two rows in reverse order: Transacti... |
| **Scenario 6: Automated Failover Split-Brain in Patroni / HAProxy PostgreSQL Cluster** | `patronictl -c /etc/patroni/config.yml list` | The high-availability cluster lacked automated STONITH (Shoot The Other Node In ... |
| **Scenario 7: Database Read-Write Splitting Fails - Read-After-Write Consistency Breakage** | `SELECT pg_current_wal_lsn();` | Read-after-write inconsistency due to asynchronous replication lag: the write qu... |
| **Scenario 8: Sharded Database Cross-Shard Query Latency Explosion** | `SELECT * FROM citus_stat_tenants ORDER BY read_count_in_past_1_hours DESC LIMIT 5;` | The application queried orders without providing the shard key (`customer_id`). ... |
| **Scenario 9: Disaster Recovery Recovery Time Objective (RTO) Failure During Point-In-Time-Recovery (PITR)** | `pgbackrest --stanza=prod --type=diff backup` | The database took full backups only once per week. Restoring to a point-in-time ... |
| **Scenario 10: Columnar Analytical Query Starvation on Transactional OLTP Cluster** | `SELECT pid, now() - query_start AS duration, query FROM pg_stat_activity WHERE query ~* 'GROUP BY' AND state != 'idle';` | Analytical queries were run directly against the row-based OLTP database, satura... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Database Reliability Engineering (DBRE) Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [AI for DevOps, AIOps & LLMOps Scenarios: Production Incidents & Triage Scenarios →](../18-AI-for-DevOps-AIOps-and-LLMOps-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

