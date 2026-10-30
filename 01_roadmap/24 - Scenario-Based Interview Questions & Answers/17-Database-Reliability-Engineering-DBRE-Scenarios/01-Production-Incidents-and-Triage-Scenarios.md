# Database Reliability Engineering (DBRE) Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: PostgreSQL Connection Pool Exhaustion & 'Too Many Clients Already' Outage

### 🚨 The Production Scenario
During a traffic surge, all application instances crash with `FATAL: remaining connection slots are reserved for non-replication superuser connections (too many clients already)`. The database becomes unresponsive.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Microservices opened direct TCP connections to PostgreSQL without pooling (e.g., 20 pods x 50 connection pool size = 1,000 connections). PostgreSQL forks a separate OS process per connection, exhausting RAM and OS context switching capacity, hitting `max_connections = 500`.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Deploy PgBouncer or Odyssey connection pooler between application and PostgreSQL in Transaction Pooling mode.
- Transaction pooling allows thousands of microservice clients to share a small, high-performance pool of 30-50 physical backend database connections.
- Tune PostgreSQL `max_connections` to a realistic hardware limit (typically 100-300 on modern multi-core servers).
- Configure client applications to release connections immediately after transaction completion.
- Monitor connection pool queue depth in Prometheus.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check active vs idle vs waiting connections in PostgreSQL
SELECT count(*), state FROM pg_stat_activity GROUP BY state;

# Display current database max connection limit
SHOW max_connections;

# Inspect PgBouncer client waiting queue and active server connections
psql -p 6432 -U pgbouncer -d pgbouncer -c 'SHOW POOLS;'

# Terminate hung backend process if necessary
kill -9 <ORPHANED_POSTGRES_BACKEND_PID>

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "PostgreSQL uses process-per-connection architecture, so high connection counts degrade CPU cache and trigger connection slot exhaustion. I resolve this by placing PgBouncer in Transaction Pooling mode in front of Postgres. Thousands of ephemeral microservice requests multiplex over just 40-50 active Postgres backend connections, slashing memory overhead and eliminating connection rejection errors."

---

## 📌 Scenario 2: PostgreSQL Transaction ID (TXID) Wraparound Catastrophic Read-Only Lockout

### 🚨 The Production Scenario
PostgreSQL begins rejecting all INSERT, UPDATE, and DELETE queries with: `FATAL: database is not accepting commands to avoid wraparound data loss in database 'appdb'`. The database is forcibly placed into read-only mode.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
PostgreSQL uses 32-bit transaction IDs. If autovacuum fails to freeze old transaction IDs before 2 billion transactions elapse, Postgres forcibly shuts down writes to prevent silent data corruption from TXID wraparound.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Stop all client applications immediately to release all connections.
- Start PostgreSQL in single-user mode (`postgres --single -D /var/lib/postgresql/data appdb`).
- Execute an emergency manual freeze vacuum: `VACUUM FREEZE ANALYZE;`.
- Restart PostgreSQL in standard multi-user mode and verify write transactions resume.
- Tune autovacuum settings: increase `autovacuum_max_workers`, lower `autovacuum_vacuum_cost_limit`, and set alert on `datfrozenxid` age exceeding 150 million transactions.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check database transaction age approaching 2-billion wraparound limit
SELECT datname, age(datfrozenxid) FROM pg_database ORDER BY age(datfrozenxid) DESC;

# Launch PostgreSQL in single-user maintenance mode
postgres --single -D /var/lib/postgresql/data -d appdb

# Run emergency manual freeze vacuum inside single-user session
VACUUM FREEZE ANALYZE VERBOSE;

# Monitor vacuum progress in real-time
SELECT pid, phase, heap_blks_scanned, heap_blks_total FROM pg_stat_progress_vacuum;

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "TXID wraparound is the ultimate database emergency. If autovacuum cannot keep up with write throughput, Postgres shuts down writes to prevent data corruption. Remediation requires booting into single-user mode to run 'VACUUM FREEZE'. To permanently avoid this, we alert when datfrozenxid age crosses 150 million and tune autovacuum cost limits so background workers vacuum aggressive tables before wraparound danger approaches."

---

## 📌 Scenario 3: MySQL Replication Lag Spikes by Hours Due to Long Single-Threaded DDL

### 🚨 The Production Scenario
A replica MySQL database used for read traffic experiences replication lag increasing from 0 seconds to 4 hours following an `ALTER TABLE` statement on the primary. Read replicas return stale data, breaking user balances.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
MySQL replica SQL thread was executing in single-threaded mode (or multi-threaded replication was partitioned poorly), and a long-running DDL statement blocked all subsequent transaction replication events in the relay log.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect replica status using `SHOW REPLICA STATUS\G` (or `SHOW SLAVE STATUS\G`).
- Enable MySQL Multi-Threaded Replication (MTS) with `replica_parallel_workers = 16` and `replica_parallel_type = LOGICAL_CLOCK`.
- Use online schema change tools (e.g., `gh-ost` or `pt-online-schema-change`) on the primary so DDL operations run without locking replicas.
- Configure client proxy (ProxySQL) to redirect read queries to primary if replica lag exceeds 5 seconds.
- Monitor `Seconds_Behind_Source` in Prometheus.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check Seconds_Behind_Source, Last_Error, and SQL thread status
SHOW REPLICA STATUS\G

# Enable parallel replication worker threads
SET GLOBAL replica_parallel_workers = 16;

# Enable parallel replication based on primary commit timestamps
SET GLOBAL replica_parallel_type = 'LOGICAL_CLOCK';

# Execute zero-downtime online schema change
gh-ost --user=root --password=xxx --host=primary --database=app --table=orders --alter='ADD COLUMN note VARCHAR(100)' --execute

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "MySQL replication lag occurs when single-threaded replay threads block on long DDL or write bursts. I fix this by configuring multi-threaded replication using LOGICAL_CLOCK, allowing the replica to replay transactions concurrently matching the primary's commit groups. Furthermore, we ban raw DDL on large tables, mandating 'gh-ost' for online, non-blocking schema evolution."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Site Reliability Engineering (SRE) & Chaos Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../16-Site-Reliability-Engineering-and-Chaos-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Database Reliability Engineering (DBRE) Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

