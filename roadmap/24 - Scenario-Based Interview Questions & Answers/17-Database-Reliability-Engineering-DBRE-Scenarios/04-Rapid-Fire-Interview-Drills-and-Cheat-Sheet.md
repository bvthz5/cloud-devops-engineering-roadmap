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

## 🔌 Database Ports & High-Availability Architecture Recall Drill

During DBRE and Systems Architecture interviews, you are expected to know every database ecosystem port, replication channel, and proxy port without hesitation:

| Port | Protocol | Database Engine / Component | Architecture Function & Interview Gotcha |
| :---: | :---: | :--- | :--- |
| **3306** | TCP | **MySQL / MariaDB Classic** | Default SQL client listener. Production databases should enforce TLS 1.3 encryption. |
| **33060** | TCP | **MySQL X Protocol** | Document Store JSON CRUD and X DevAPI. Often disabled if only classic SQL is used. |
| **33061** | TCP | **MySQL Group Replication** | Internal Paxos group consensus port for MySQL InnoDB Clusters. Never expose publicly. |
| **33062** | TCP | **MySQL Admin Interface** | Dedicated emergency port for DBAs to log in when connection pools exhaust 3306. |
| **4444** | TCP | **Galera SST (State Transfer)** | Full binary snapshot sync using `mariabackup` or `rsync` when a new Galera node boots. |
| **4567** | TCP / UDP | **Galera Cluster Replication** | wsrep write-set replication and multicast/unicast group membership heartbeats. |
| **4568** | TCP | **Galera IST (Incremental Transfer)**| Incremental cache transfer for nodes that briefly disconnected. |
| **5432** | TCP | **PostgreSQL** | Primary client database port. Apps should route through PgBouncer rather than connecting directly. |
| **6032** | TCP | **ProxySQL Admin** | SQLite management console to configure hostgroups, query rules, and users live without restarts. |
| **6033** | TCP | **ProxySQL Traffic** | Application-facing proxy port providing seamless read-write splitting and connection pooling. |
| **6379** | TCP | **Redis Server** | In-memory key-value cache and pub/sub. Production requires `protected-mode yes` and VPC peering. |
| **6432** | TCP | **PgBouncer** | PostgreSQL connection pooler (Transaction Mode) preventing process fork memory exhaustion. |
| **7000 / 7001** | TCP | **Cassandra Inter-Node** | Unencrypted (7000) and TLS (7001) inter-node Cassandra storage gossip communication. |
| **8008** | TCP | **Patroni REST API** | Leader election, DCS health probes, and failover switchover endpoint for HAProxy. |
| **8123** | TCP | **ClickHouse HTTP** | High-performance OLAP query interface used by BI tools, Grafana, and HTTP clients. |
| **9000** | TCP | **ClickHouse Native TCP** | Binary protocol for `clickhouse-client` and multi-threaded data bulk ingestion. |
| **9004 / 9005** | TCP | **ClickHouse MySQL/Postgres Wire** | Emulation listeners allowing standard `mysql` and `psql` clients to query ClickHouse. |
| **9009** | TCP | **ClickHouse Replication** | Inter-server distributed partition and part replication port. |
| **9042** | TCP | **Cassandra / ScyllaDB (CQL)** | Native binary transport port for Cassandra Query Language database queries. |
| **9200** | TCP | **Elasticsearch / OpenSearch REST** | JSON search and indexing endpoint. |
| **9300** | TCP | **Elasticsearch Cluster Transport**| Internal binary node-to-node discovery and shard relocation transport port. |
| **16379** | TCP | **Redis Cluster Bus** | Node-to-node gossip, cluster topology updates, and slot migration (`Client Port + 10,000`). |
| **26257** | TCP | **CockroachDB SQL & Gossip** | Unified SQL client protocol and multi-node Raft consensus port. |
| **26379** | TCP | **Redis Sentinel** | High-availability Sentinel monitor port for automatic master failover election. |
| **27017** | TCP | **MongoDB mongod / mongos** | Standard client connection port for MongoDB databases and sharding routers. |
| **27018** | TCP | **MongoDB Shard Instance** | Default listener when running a mongod daemon as a dedicated cluster shard member. |
| **27019** | TCP | **MongoDB Config Server** | Metadata and routing chunk distribution storage for sharded cluster configurations. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Database Reliability Engineering (DBRE) Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [AI for DevOps, AIOps & LLMOps Scenarios: Production Incidents & Triage Scenarios →](../18-AI-for-DevOps-AIOps-and-LLMOps-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

