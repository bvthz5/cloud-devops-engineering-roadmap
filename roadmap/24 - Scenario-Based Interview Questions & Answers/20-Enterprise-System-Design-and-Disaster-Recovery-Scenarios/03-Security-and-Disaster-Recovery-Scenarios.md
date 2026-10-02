# Enterprise System Design & Disaster Recovery Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Distributed Cache-Aside Race Condition & Dirty Reads in Financial Ledger

### 🚨 The Production Scenario
In a distributed banking application using Cache-Aside (Redis + PostgreSQL), concurrent balance transfers cause user balances in Redis to show $500 while the PostgreSQL database holds $200. Users withdraw funds based on the stale cached balance.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A race condition between database update and cache invalidation: Thread A updated the database and invalidated the cache; Thread B concurrently read stale data from the database before Thread A committed, and then populated Redis with the stale data.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Implement 'Cache Invalidation with Transaction Commit Hooks': only invalidate the cache AFTER the database transaction commits successfully.
- Use 'Cache Eviction over Cache Update': never update the cache with new values directly; always DELETE the cache key so subsequent reads fetch fresh committed data.
- Implement Delayed Double Deletion: delete the cache key immediately, and delete it again 500ms later to catch any concurrent stale reads that slipped through.
- For mission-critical ledger records, bypass cache entirely and read directly from PostgreSQL using `SELECT ... FOR UPDATE`.
- Audit ledger data consistency with reconciliation jobs.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Delete stale cache key immediately
redis-cli del user_balance:12345

# Execute pessimistic locking query in PostgreSQL to read true authoritative balance
SELECT balance FROM accounts WHERE user_id = 12345 FOR UPDATE;

# Monitor real-time Redis cache read/write operations
redis-cli monitor | grep user_balance

# Execute automated ledger vs cache reconciliation audit
python3 run_balance_reconciliation.py --mismatch-threshold 0

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Cache-Aside race conditions occur when a stale read writes to Redis after a database commit. I eliminate this with three principles: First, always delete cache keys rather than updating them. Second, execute cache invalidation in an after-commit transaction hook, followed by a delayed double-delete 500ms later. Third, for financial balances, we bypass cache entirely and enforce transactional pessimistic row locks."

---

## 📌 Scenario 8: API Gateway DDoS Protection Saturation - Layer 7 Slowloris Attack

### 🚨 The Production Scenario
An enterprise API gateway experiences connection pool exhaustion. Legitimate users receive connection timeouts, but network bandwidth and CPU usage remain low (under 15%).

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A Layer 7 Slowloris DDoS attack: attackers opened thousands of HTTP connections and sent partial request headers extremely slowly (e.g., 1 byte every 10 seconds), holding server connection sockets open indefinitely until the server ran out of file descriptors.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Deploy Cloudflare, AWS CloudFront, or AWS Shield at the edge to absorb and terminate Layer 7 connections before reaching the origin.
- Configure edge web servers (Nginx / Envoy): enforce strict `client_header_timeout` (e.g., 5 seconds) and `client_body_timeout`.
- Limit the maximum time allowed for request headers to complete (`client_header_buffer_size`).
- Enforce connection rate limits per source IP using IP reputation filters.
- Verify open file descriptors and connection limits in Linux kernel sysctl.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Count half-open and slow connection states
ss -tan state syn-recv | wc -l

# Identify top IP addresses holding open connections
netstat -an | grep :443 | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -n | tail -n 10

# Apply AWS WAF rate-limiting rule
aws wafv2 update-web-acl --name prod-waf --scope CLOUDFRONT --rules file://slowloris-mitigation.json

# Tune kernel TCP SYN backlog queue
sysctl -w net.ipv4.tcp_max_syn_backlog=4096

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Slowloris is a stealthy Layer 7 attack that starves web servers without generating high bandwidth by sending trickle headers. I mitigate this by terminating all TLS connections at Cloudflare or AWS CloudFront edge before they touch our infrastructure. On our origin proxies, we enforce strict client_header_timeout limits (5s) so any connection that does not complete headers promptly is severed immediately."

---

## 📌 Scenario 9: Bulk Data Ingestion Pipeline Memory Exhaustion via Unbounded Outbox Pattern

### 🚨 The Production Scenario
A transactional outbox worker pulling events from PostgreSQL and publishing to Apache Kafka crashes repeatedly with OOMKilled. The `outbox_events` table accumulates 15 million un-processed rows.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The outbox polling query executed `SELECT * FROM outbox_events WHERE status = 'pending' FOR UPDATE` without a `LIMIT` clause. As transaction volume grew, the query loaded millions of rows into Java heap memory at once, crashing the JVM.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Refactor the outbox poller to process in fixed micro-batches: `SELECT * FROM outbox_events WHERE status = 'pending' ORDER BY id ASC LIMIT 500 FOR UPDATE SKIP LOCKED`.
- `SKIP LOCKED` allows multiple parallel outbox worker pods to process distinct chunks concurrently without lock contention.
- Transition from periodic polling to Log-Based Change Data Capture (Debezium): Debezium reads WAL logs directly with zero database polling overhead.
- Implement partitioning on the outbox table by day/week and drop processed partitions.
- Scale Kafka producer throughput.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check backlog of un-processed outbox messages
SELECT count(*), status FROM outbox_events GROUP BY status;

# Execute safe micro-batch outbox query with non-blocking concurrency
SELECT * FROM outbox_events WHERE status = 'pending' ORDER BY id ASC LIMIT 500 FOR UPDATE SKIP LOCKED;

# Scale outbox worker pods horizontally
kubectl scale deployment outbox-worker -n prod --replicas=5

# Check Debezium CDC outbox connector health
curl -s http://debezium-connect:8083/connectors/outbox-connector/status | jq .

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Polling outbox tables without batch limits is an OOM trap. I fix this by using 'LIMIT 500 FOR UPDATE SKIP LOCKED', which enables multiple worker pods to process distinct records concurrently without lock contention. For high-scale architectures, I eliminate polling entirely by deploying Debezium CDC, which streams database changes directly from PostgreSQL WAL logs into Kafka with zero query overhead."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Enterprise System Design & Disaster Recovery Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Enterprise System Design & Disaster Recovery Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

