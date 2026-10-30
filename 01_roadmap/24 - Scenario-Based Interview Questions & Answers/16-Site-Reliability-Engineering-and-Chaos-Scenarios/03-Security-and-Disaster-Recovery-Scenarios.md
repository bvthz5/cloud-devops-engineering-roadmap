# Site Reliability Engineering (SRE) & Chaos Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: SRE Capacity Planning - Modeling Black Friday 10x Load Spikes

### 🚨 The Production Scenario
An enterprise faces its annual holiday shopping event where traffic surges 10x in 60 seconds. Previous years suffered crashes due to database connection limits and third-party API rate limits.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Capacity planning was done through guesswork rather than synthetic load testing, architectural bottleneck modeling, and pre-scaling critical infrastructure.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Conduct synthetic load testing using Locust, k6, or Distributed Gatling up to 15x peak historical load.
- Model the critical user path: identify bottlenecks in database connection pools, cache memory, and network throughput.
- Pre-scale compute node pools, databases, and caches 24 hours prior to the event to eliminate autoscaling lag.
- Implement request queueing and virtual waiting rooms (e.g., Cloudflare Waiting Room) to meter traffic at the edge.
- Coordinate with upstream third-party payment providers to temporarily raise API TPS quotas.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Execute distributed high-concurrency load test with k6
k6 run --vus 5000 --duration 30m load-test-script.js

# Pre-scale application replicas in advance of traffic surge
kubectl scale deployment order-service -n prod --replicas=100

# Pre-scale database instance tier for event
aws rds modify-db-instance --db-instance-identifier db-prod --db-instance-class db.r6g.8xlarge --apply-immediately

# Verify Redis cache memory headroom under simulated load
redis-cli info memory | grep used_memory_human

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Autoscaling cannot react fast enough to a 10x surge in 60 seconds. SRE capacity planning requires pre-scaling: we load test with k6 to 15x projected volume, pre-warm databases and cache clusters, and set baseline pod replicas hours before the event. At the perimeter, we configure Cloudflare Waiting Rooms as an insurance policy to queue excess users gracefully if traffic exceeds all models."

---

## 📌 Scenario 8: Thundering Herd Problem - Cache Invalidation Triggers Complete Database Death

### 🚨 The Production Scenario
A high-traffic homepage Redis cache key storing product listings expires at 12:00 PM. In the next 200 milliseconds, 50,000 concurrent requests miss the cache and hit the PostgreSQL database simultaneously, crashing the database within seconds.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The 'Thundering Herd' (cache stampede) effect: multiple concurrent requests experienced a cache miss at the exact same moment and all attempted to regenerate the cache by querying the primary database concurrently.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Implement Mutex / Distributed Locking (e.g., Redlock) in the application: only the first thread that misses the cache queries the database, while other threads wait for the cache to populate.
- Implement Probabilistic Early Expiration (XFetch algorithm) to refresh the cache asynchronously in the background before it actually expires.
- Add randomized jitter to cache TTLs (e.g., `TTL = 3600 + rand(0, 300)`) to prevent multiple keys from expiring simultaneously.
- Serve stale cache data during backend regeneration rather than allowing direct database hits.
- Restart database and warm the critical cache keys using a warm-up script.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Test acquiring distributed mutex lock in Redis
redis-cli set product_catalog_lock 1 NX EX 10

# Inspect remaining TTL of critical cache keys
redis-cli ttl homepage_products

# Execute background cache warming script
python3 warmup_cache.py --endpoint https://internal-api.corp/cache/warm

# Identify identical queries flooding database during stampede
SELECT count(*), query FROM pg_stat_activity WHERE state != 'idle' GROUP BY query ORDER BY count(*) DESC;

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Cache stampedes kill databases when high-traffic keys expire without locks. I prevent this using two patterns: first, distributed mutex locking so only a single worker queries the database on a cache miss while others wait; second, background probabilistic refreshing (XFetch) that updates the cache before the TTL expires, ensuring users ALWAYS hit a warm cache and the database never sees a surge."

---

## 📌 Scenario 9: GameDay Disaster Recovery - Live Data Center Cutover Drill

### 🚨 The Production Scenario
During an unannounced GameDay drill simulating a total outage of the primary data center (DC-East), the automated failover system triggers, but DNS caching on internet ISP resolvers prevents 40% of users from switching to DC-West for over 4 hours.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The DNS A/CNAME records had a Time-To-Live (TTL) set to 86,400 seconds (24 hours). Even though Route 53 updated its records, ISP DNS resolvers cached the old failed IP address.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Standardize DNS TTL for all critical production endpoints to 60 seconds at all times.
- Adopt Anycast IP routing (e.g., AWS Global Accelerator or Cloudflare) rather than raw DNS switching: Anycast keeps the public IP identical while BGP routes traffic to the surviving DC in seconds.
- Verify database state replication: ensure secondary database in DC-West was fully caught up before cutover.
- Conduct automated health check probing from independent external regions.
- Document failover timeline and RTO/RPO metrics achieved during the drill.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect public DNS resolution and current TTL values
dig www.example.com +nostats +nocmd

# Check Anycast Global Accelerator status and health checks
aws globalaccelerator list-accelerators

# Set aggressive 60s TTL on DNS records
aws route53 change-resource-record-sets --hosted-zone-id Z123 --change-batch '{"Changes":[{"Action":"UPSERT","ResourceRecordSet":{"Name":"app.corp.com","Type":"A","TTL":60,"ResourceRecords":[{"Value":"198.51.100.1"}]}}]}'

# Trace routing path and verify traffic terminates at healthy data center
mtr -rw www.example.com

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Relying on raw DNS for disaster recovery is flawed because external recursive DNS resolvers routinely ignore low TTLs. For true sub-minute RTO, I implement Anycast routing via AWS Global Accelerator or Cloudflare. Clients connect to a static Anycast IP that never changes; when a data center fails, BGP withdraws the route, redirecting traffic to the healthy region in under 10 seconds."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Site Reliability Engineering (SRE) & Chaos Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Site Reliability Engineering (SRE) & Chaos Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

