# 04 - Configuration and Rules: PostgreSQL & MySQL Networking & Tuning

In enterprise Database Reliability Engineering (DBRE), configuring database network listeners, ports, connection pools, and replication streams correctly is the difference between seamless high availability and split-brain outages.

---

## 1. Complete MySQL & MariaDB Networking & Ports Matrix

MySQL supports multiple specialized networking protocols and listening ports depending on whether the server operates standalone, behind a proxy, in an asynchronous replica set, or inside a synchronous Galera / InnoDB Cluster:

```text
                                  ┌─────────────────────────────┐
                                  │   Application Pods / ORMs   │
                                  └──────────────┬──────────────┘
                                                 │
                                                 ▼
                                  ┌─────────────────────────────┐
                                  │   ProxySQL (Port 6033)      │
                                  │   Admin Interface (6032)    │
                                  └──────────────┬──────────────┘
                                                 │
                   ┌─────────────────────────────┼─────────────────────────────┐
                   ▼                             ▼                             ▼
       ┌───────────────────────┐     ┌───────────────────────┐     ┌───────────────────────┐
       │   MySQL Node 1        │     │   MySQL Node 2        │     │   MySQL Node 3        │
       │   Classic SQL: 3306   │     │   Classic SQL: 3306   │     │   Classic SQL: 3306   │
       │   X-Protocol:  33060  │     │   X-Protocol:  33060  │     │   X-Protocol:  33060  │
       │   Admin Port:  33062  │     │   Admin Port:  33062  │     │   Admin Port:  33062  │
       │   Group Repl:  33061  │◄───►│   Group Repl:  33061  │◄───►│   Group Repl:  33061  │
       └───────────────────────┘     └───────────────────────┘     └───────────────────────┘
```

| Port Number | Protocol | MySQL Component | Production Purpose & Configuration Directive |
| :---: | :---: | :--- | :--- |
| **3306** | TCP | **MySQL Classic SQL Protocol** | Standard client connection port. Set via `port = 3306` and `bind-address = 0.0.0.0` in `my.cnf`. |
| **33060** | TCP | **MySQL X Protocol** | X DevAPI for asynchronous Document Store JSON collections. Controlled via `mysqlx = ON` and `mysqlx_port = 33060`. |
| **33061** | TCP | **Group Replication (MGR)** | Multi-master/Single-master Paxos group communication: `group_replication_local_address = "node1.corp:33061"`. |
| **33062** | TCP | **MySQL Admin Interface** | Dedicated emergency maintenance channel when `max_connections` is exhausted: `admin_port = 33062`, `admin_address = "127.0.0.1"`. |
| **6032** | TCP | **ProxySQL Admin** | SQLite-compatible management interface for configuring users, backends, and query rules (`mysql -u admin -p -h 127.0.0.1 -P 6032`). |
| **6033** | TCP | **ProxySQL Client Traffic** | High-performance client-facing listener providing transparent connection multiplexing and read/write splitting. |
| **4567** | TCP / UDP | **Galera Replication** | Galera wsrep multi-master group replication traffic, state change broadcasts, and multicast/unicast heartbeats. |
| **4568** | TCP | **Galera IST** | Incremental State Transfer port. Transfers missing transaction write-sets from donor node cache during transient disconnects. |
| **4444** | TCP | **Galera SST** | State Snapshot Transfer port. Executes full physical block sync via `mariabackup` or `rsync` when re-provisioning a node. |
| **1186** | TCP | **MySQL NDB Management** | Cluster management server daemon (`ndb_mgmd`) control and arbitration channel. |
| **2202** | TCP | **MySQL NDB Data Node** | Inter-node data transport between high-availability in-memory NDB storage cluster nodes. |
| **15306** | TCP | **Vitess VTGate (MySQL)** | Vitess distributed sharding proxy listener mimicking standard MySQL protocol for horizontal database sharding. |
| **15000** | TCP | **Vitess HTTP Status** | Web-based diagnostic, status, and health-check monitoring dashboard for VTGate and VTTablet instances. |

---

## 2. Complete PostgreSQL Networking & Ports Matrix

PostgreSQL utilizes a clean, modular process-per-connection architecture backed by high-concurrency connection poolers and orchestration daemons:

| Port Number | Protocol | PostgreSQL Component | Architectural Role & Configuration Directive |
| :---: | :---: | :--- | :--- |
| **5432** | TCP | **PostgreSQL Client Protocol** | Primary client database connection port. Configured via `port = 5432` and `listen_addresses = '*'` in `postgresql.conf`. |
| **5433** | TCP | **PostgreSQL Secondary / Citus** | Common port convention for running secondary instances on the same host, or Citus distributed coordinator nodes. |
| **6432** | TCP | **PgBouncer Connection Pooler** | Transaction, Session, or Statement pooling gateway: `listen_port = 6432`, `listen_addr = *`, `pool_mode = transaction`. |
| **8008** | TCP | **Patroni REST API** | Distributed consensus leader election, DCS health probes, and automated failover switchover endpoint for HAProxy/Keepalived. |
| **8432** | TCP | **Odyssey Connection Pooler** | Advanced multi-threaded asynchronous connection pooler optimized for 50,000+ client connections. |
| **26257** | TCP | **CockroachDB (Postgres Wire)** | PostgreSQL-compatible distributed SQL client listener and Raft consensus inter-node transport. |
| **5433** | TCP | **YugabyteDB YSQL** | Distributed SQL layer supporting 100% native PostgreSQL wire protocol and ACID transactions. |

---

## 3. Production Configuration Best Practices for DBREs

### 🔧 MySQL Production `my.cnf` Network & Connection Optimization
```ini
[mysqld]
# Network Binding
bind-address                 = 0.0.0.0
port                         = 3306

# Dedicated Emergency DBA Rescue Port
admin_address                = 127.0.0.1
admin_port                   = 33062

# Connection Limits & Timeouts
max_connections              = 1000
max_connect_errors           = 100000
connect_timeout              = 10
wait_timeout                 = 28800
interactive_timeout          = 28800
back_log                     = 512

# Buffer & I/O Scaling
innodb_buffer_pool_size      = 24G        # 70-80% of total host RAM
innodb_buffer_pool_instances  = 8
innodb_flush_log_at_trx_commit = 1        # ACID Guarantee (RPO = 0)
innodb_flush_method          = O_DIRECT
```

### 🐘 PostgreSQL Production `postgresql.conf` Network & Connection Optimization
```ini
# Network Binding
listen_addresses             = '*'
port                         = 5432

# Connection Allocation (Keep low when using PgBouncer!)
max_connections              = 200
superuser_reserved_connections = 5

# Memory & Buffer Management
shared_buffers               = 16GB       # 25% of total host RAM
effective_cache_size         = 48GB       # 75% of total host RAM
work_mem                     = 64MB       # Per sorting operation
maintenance_work_mem         = 2GB

# Replication & WAL Safeguards (RPO = 0)
wal_level                    = replica
max_wal_senders              = 10
wal_keep_size                = 4GB
synchronous_commit           = on
```

---

## 4. DBRE Socket Inspection & Triage Commands

```bash
# 1. Check all listening database ports across MySQL, PostgreSQL, and Redis
ss -tlpn | grep -E ':(3306|33060|33061|33062|5432|6432|6379|8008)'

# 2. Check how many active connections are established on MySQL port 3306
ss -tan 'sport = :3306' | grep -c ESTAB

# 3. Emergency connect to MySQL via the dedicated admin port when 3306 is saturated
mysql -u root -p -h 127.0.0.1 -P 33062

# 4. Probe remote database reachability with netcat (1 second timeout)
nc -zvw1 db-prod.corp.internal 3306
nc -zvw1 db-prod.corp.internal 5432

# 5. Measure network round-trip latency to database port using tcping
nping --tcp -p 3306 db-prod.corp.internal -c 4
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Installation and Setup](./03-Installation-and-Setup.md) | [Index](../../../README.md) | [05 - Advanced Techniques →](./05-Advanced-Techniques.md) |

