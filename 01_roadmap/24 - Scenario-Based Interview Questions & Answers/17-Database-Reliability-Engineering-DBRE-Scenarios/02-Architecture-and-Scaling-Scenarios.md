# Database Reliability Engineering (DBRE) Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Redis Cluster OOM and Key Eviction Storm on Unset TTL Data

### 🚨 The Production Scenario
A production Redis cluster hits `maxmemory: 32gb`. Redis begins evicting thousands of critical user session keys per second. Application users are logged out en masse.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Redis `maxmemory-policy` was set to `allkeys-lru` or `allkeys-lfu`. A new feature stored permanent analytics events without an expiration TTL, filling RAM until Redis began evicting active session keys to make room for non-expiring data.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Identify keys consuming memory using `redis-cli --bigkeys` and Redis MEMORY USAGE.
- Change eviction policy to `volatile-lru` so Redis only evicts keys with an explicit TTL, rejecting writes for keys without TTL (`OOM command not allowed`).
- Purge the non-critical unexpired analytics keys.
- Separate workloads into distinct Redis instances: dedicated Redis for caching/sessions and separate datastore for persistent data.
- Enforce a strict TTL policy on all caching libraries.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Scan Redis keyspace to find largest keys and types
redis-cli --bigkeys

# Inspect active key eviction policy
redis-cli config get maxmemory-policy

# Switch eviction policy to protect keys without TTL
redis-cli config set maxmemory-policy volatile-lru

# Inspect used_memory vs maxmemory and eviction counts
redis-cli info memory

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Using allkeys-lru on a Redis instance shared between cache and persistent data is dangerous because cache insertions will evict active sessions. I immediately switch maxmemory-policy to volatile-lru to protect non-expiring keys, purge the offending un-expiring cache keys, and architecturally separate session caches from analytical stores into distinct Redis clusters."

---

## 📌 Scenario 5: Database Deadlocks Paralyze E-Commerce Inventory Checkout

### 🚨 The Production Scenario
During high concurrency checkout, application logs record hundreds of `ERROR: deadlock detected` in PostgreSQL. Orders fail and database CPU surges due to lock contention.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Two concurrent transactions locked the same two rows in reverse order: Transaction A updated Product 1 then Product 2; Transaction B updated Product 2 then Product 1. PostgreSQL detected the circular lock dependency and aborted one transaction.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect deadlock details in PostgreSQL server logs or `pg_stat_database.deadlocks`.
- Enforce deterministic lock acquisition ordering in application code: always sort row IDs (e.g., `ORDER BY product_id ASC`) before executing `SELECT ... FOR UPDATE`.
- Keep transactions as short as possible: eliminate network calls or slow computations inside database transaction blocks.
- Implement exponential backoff and retry in the application for deadlock exceptions.
- Tune `deadlock_timeout` (default 1s) to balance lock detection speed and CPU overhead.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check total deadlock count on database
SELECT * FROM pg_stat_database WHERE datname = 'appdb';

# Extract full deadlock transaction graph and offending SQL statements from logs
grep -i 'deadlock detected' /var/log/postgresql/postgresql-*.log

# Check deadlock detection interval
SHOW deadlock_timeout;

# Tune deadlock detection speed
SET deadlock_timeout = '500ms';

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Deadlocks happen when concurrent transactions acquire locks in inconsistent orders. The definitive fix is deterministic lock ordering: if every transaction always locks product IDs in sorted numerical order, circular lock dependencies become mathematically impossible. We also keep database transactions short and implement smart SDK retries for transient conflicts."

---

## 📌 Scenario 6: Automated Failover Split-Brain in Patroni / HAProxy PostgreSQL Cluster

### 🚨 The Production Scenario
Following a transient network partition between data centers, both the old primary and a promoted secondary claim to be the read-write primary, accepting writes from different application pods and causing data divergence.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The high-availability cluster lacked automated STONITH (Shoot The Other Node In The Head) / fencing mechanisms. The old primary did not recognize it was partitioned before the secondary acquired the distributed consensus lock in etcd.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately shut down the demoted old primary node to stop divergent writes.
- Deploy Patroni with distributed consensus store (etcd or Consul) enforcing Watchdog / hardware fencing (`/dev/watchdog`).
- In Patroni, if a primary fails to refresh its lease in etcd within `ttl` seconds, the Linux kernel watchdog automatically hard-resets the node before another node can be promoted.
- Compare WAL positions and use `pg_rewind` to safely resynchronize the old primary as a replica of the new primary.
- Verify HAProxy routes write traffic strictly to the active Patroni primary leader.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect Patroni cluster topology, leader, and replication state
patronictl -c /etc/patroni/config.yml list

# Pause Patroni automated failover during emergency reconciliation
patronictl -c /etc/patroni/config.yml pause

# Rewind divergent old primary to match new primary timeline
pg_rewind -D /var/lib/postgresql/data --source-server='host=new-primary port=5432 user=postgres'

# Probe Patroni REST API endpoint for leader validation
curl http://localhost:8008/primary

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Split-brain in database HA leads to silent data divergence. I prevent this using Patroni with etcd consensus and Linux hardware watchdog fencing. If the primary loses its etcd lease for more than 10 seconds, the kernel watchdog triggers an immediate hardware reset before any standby can promote, guaranteeing exactly-one active writer at all times."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Database Reliability Engineering (DBRE) Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Database Reliability Engineering (DBRE) Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

