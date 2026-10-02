# Database Reliability Engineering (DBRE) Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Database Read-Write Splitting Fails - Read-After-Write Consistency Breakage

### 🚨 The Production Scenario
After placing ProxySQL or PgBouncer in front of a primary-replica cluster to offload read traffic, users update their profile, but upon page reload, their old profile data displays. Reloading again displays the updated profile.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Read-after-write inconsistency due to asynchronous replication lag: the write query completed on the primary, but the subsequent read query was routed to a read replica that had not yet applied the replication WAL stream.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Implement 'Sticky Reads / Session Pinning' in the database proxy or application ORM.
- When a user performs a write transaction, pin all read queries for that specific user session to the primary database for the next N seconds (e.g., 5 seconds) before routing to replicas.
- Utilize GTID (Global Transaction Identifier) / LSN (Log Sequence Number) causal consistency tracking: client passes the write LSN in request headers; replica only serves the read if its replay LSN >= request LSN.
- Route critical reads (e.g., balance, checkout) strictly to the primary.
- Monitor replication lag closely.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Get primary current write WAL position LSN
SELECT pg_current_wal_lsn();

# Get replica current replay LSN position
SELECT pg_last_wal_replay_lsn();

# Calculate exact replication lag in bytes
SELECT pg_wal_lsn_diff(pg_current_wal_lsn(), pg_last_wal_replay_lsn());

# Inspect ProxySQL read-write splitting query rules
mysql -u proxysql -p -P 6032 -e 'SELECT * FROM mysql_query_rules;'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Asynchronous replicas are eventually consistent, which breaks user expectations if read-after-write is not handled. I resolve this by enforcing session stickiness: whenever a user writes, we pin their reads to the primary for 5 seconds. For higher fidelity, we use causal consistency with LSN/GTID tokens, ensuring a replica only serves the read if it has replayed that user's specific commit."

---

## 📌 Scenario 8: Sharded Database Cross-Shard Query Latency Explosion

### 🚨 The Production Scenario
An e-commerce platform sharded across 16 PostgreSQL shards experiences query latency jumping from 10ms to 4,500ms on order history pages, saturating network bandwidth between database nodes.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The application queried orders without providing the shard key (`customer_id`). The sharding middleware (e.g., Citus or Vitess) was forced to execute a scatter-gather query across all 16 shards, buffering and merging millions of rows in application memory.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Audit slow query logs to identify scatter-gather queries lacking the partition/shard key in WHERE clauses.
- Enforce routing by shard key: refactor application queries to always include `customer_id` so the coordinator routes directly to a single shard.
- For queries that require multi-shard aggregation (e.g., reporting), offload them to an analytical datastore (ClickHouse, Snowflake, BigQuery) via Change Data Capture (CDC / Debezium).
- Ban unbounded joins across distributed tables.
- Monitor shard distribution and coordinator query execution plans.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect Citus distributed shard tenant activity
SELECT * FROM citus_stat_tenants ORDER BY read_count_in_past_1_hours DESC LIMIT 5;

# Inspect distributed query plan showing Scatter-Gather across all shards
EXPLAIN (VERBOSE) SELECT * FROM orders WHERE status = 'shipped';

# Verify single-shard direct route plan
EXPLAIN (VERBOSE) SELECT * FROM orders WHERE customer_id = 12345;

# Inspect physical shard placements
SELECT shardid, shardstate FROM pg_dist_shard_placement;

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "In sharded databases, scatter-gather queries destroy performance because the slowest shard dictates latency and coordinator memory explodes. I enforce that all OLTP queries include the shard key to achieve single-shard routing. For cross-entity searches and analytics, we replicate data via Kafka CDC to ClickHouse, keeping the sharded transactional cluster lean."

---

## 📌 Scenario 9: Disaster Recovery Recovery Time Objective (RTO) Failure During Point-In-Time-Recovery (PITR)

### 🚨 The Production Scenario
A developer runs an accidental `DROP TABLE users;` in production. The DBRE team initiates Point-In-Time-Recovery (PITR) to restore to 1 minute prior to the drop. The restore takes 14 hours instead of the promised 30-minute RTO, violating enterprise SLAs.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The database took full backups only once per week. Restoring to a point-in-time required downloading a 4TB base backup from object storage and sequentially replaying 6 days of WAL archive logs over a single CPU core.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Increase full base backup frequency (e.g., daily full or differential backups).
- Use parallel backup and restore tools like `pgBackRest` or Percona XtraBackup supporting multi-core compression and parallel WAL replay.
- Deploy continuous standby delay replicas: maintain an automated replica running 1 hour behind real-time so recovery takes minutes rather than hours.
- Automate periodic synthetic PITR drills in CI to continuously validate actual RTO and RPO metrics.
- Implement safety guardrails: drop table protections, read-only roles, and dbt migration checks.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Execute differential backup with multi-thread compression
pgbackrest --stanza=prod --type=diff backup

# Execute fast parallel Point-In-Time Recovery
pgbackrest --stanza=prod --target="2026-10-02 14:30:00" --target-action=promote restore

# Validate integrity of WAL archives and base backups
pgbackrest --stanza=prod check

# Check size of WAL archives awaiting replay
du -sh /var/lib/postgresql/wal_archive

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "PITR takes too long when you must replay days of WAL logs from a weekly base backup. I optimize RTO by switching to pgBackRest with daily differential backups and multi-threaded parallel replay. Furthermore, for mission-critical databases, we maintain a dedicated 'delayed replica' running 1 hour behind production. If someone drops a table, we promote the delayed replica in under 5 minutes."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Database Reliability Engineering (DBRE) Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Database Reliability Engineering (DBRE) Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

