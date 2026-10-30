# Enterprise System Design & Disaster Recovery Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Regional Blackout Recovery - RTO < 5 Minutes, RPO < 1 Minute Across Regions

### 🚨 The Production Scenario
A catastrophic physical power loss takes down AWS `us-east-1`. Enterprise SLAs mandate RTO < 5 minutes and RPO < 1 minute for all core banking services.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Failover failed in past drills because secondary infrastructure was deployed cold, and database cross-region replication lag was unmonitored and exceeded 10 minutes.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Maintain a 'Warm Standby' or 'Hot Standby' infrastructure in secondary region (`us-west-2`) via continuous GitOps synchronization.
- Utilize Aurora Global Database or DynamoDB Global Tables which achieve typical cross-region storage replication lag under 1 second (RPO < 1s).
- Configure AWS Route 53 Application Recovery Controller (ARC) with automated health routing controls.
- When us-east-1 fails, Route 53 ARC detects the outage and flips the routing control to us-west-2.
- Aurora Global Database is promoted to primary via automated API call, restoring read-write capabilities in under 90 seconds (RTO < 2 minutes).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Promote secondary Aurora cluster to standalone primary
aws rds failover-global-cluster --global-cluster-identifier global-db --target-db-cluster-identifier-arn arn:aws:rds:us-west-2:123:cluster:aurora-west

# Flip Route 53 ARC routing controls from primary to secondary region
aws route53-recovery-control-config update-routing-control-states --routing-control-states-entries "RoutingControlArn=arn:aws:route53-recovery-control::123:control/xxx,RoutingControlState=Off" "RoutingControlArn=arn:aws:route53-recovery-control::123:control/yyy,RoutingControlState=On"

# Check Route 53 global health check status
aws route53 get-health-check-status --health-check-id <ID>

# Validate secondary region service health
curl -s https://api-west.corp.com/healthz

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Achieving RTO under 5 minutes and RPO under 1 minute requires Aurora Global Database paired with Route 53 Application Recovery Controller. Aurora handles sub-second storage-level physical replication across regions, meeting our RPO. For RTO, Route 53 ARC decouples routing from standard DNS TTLs, executing atomic regional traffic flips and promoting the secondary cluster in under 90 seconds."

---

## 📌 Scenario 5: Global Anycast BGP Route Leaks Cause Latency Spikes for 50% of Global Users

### 🚨 The Production Scenario
Users in Europe and North America report that accessing an enterprise web application is routing through an ISP in Southeast Asia, adding 350ms of network latency and dropping packets.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A transit ISP accidentally leaked BGP autonomous system (AS) routes, advertising a more specific prefix or preferred path for the enterprise Anycast IP range, hijacking global BGP routing tables.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Use global BGP monitoring tools (e.g., ThousandEyes, RIPE Atlas, Catchpoint) to identify the leaking autonomous system (ASN).
- Enforce Resource Public Key Infrastructure (RPKI) Route Origin Authorization (ROA) on all public IP prefixes.
- With RPKI ROA enforced, upstream Tier-1 transit providers automatically drop invalid and unauthorized BGP route announcements.
- Work with upstream network carriers to implement BGP community filtering and reject leaked routes.
- Verify BGP route convergence restores optimal Anycast routing.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect RADB route object and originating ASN registration
whois -h whois.radb.net '198.51.100.0/24'

# Validate RPKI cryptographic certificates and ROA validity
rpki-client -v

# Run network diagnostic report displaying ASN numbers per hop
mtr -rwz 198.51.100.1

# Query RIPE global BGP routing status API
curl -s https://stat.ripe.net/data/routing-status/data.json?resource=198.51.100.0/24 | jq .

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "BGP route leaks and hijacking occur when transit providers accept unverified route announcements. I defend our global Anycast perimeter by implementing RPKI Route Origin Authorization (ROA). Major Tier-1 transit carriers enforce RPKI validation; if a rogue ASN advertises our IP space, Tier-1 providers flag it as RPKI-Invalid and drop it immediately, preventing global traffic diversion."

---

## 📌 Scenario 6: Microservices Circuit Breaker Thundering Herd Post-Outage Recovery

### 🚨 The Production Scenario
A downstream payment service that was down for 30 minutes recovers and comes back online. The moment its health checks pass, 20 upstream microservices close their circuit breakers and bombard it with 100,000 queued requests, instantly crashing it again.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Circuit breakers lacked a 'Half-Open' rate-limiting throttle and retry requests were not smoothed. When the circuit flipped from Open to Closed, all accumulated client requests hit simultaneously without a warm-up ramp.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Configure Circuit Breakers with a strict 'Half-Open' state: allow only a tiny trickle of requests (e.g., 5% or 10 req/s) to probe backend health.
- Implement Exponential Backoff with Full Jitter on all client retry queues to spread the request load across time.
- Enforce rate limiting and concurrency limits at the payment service ingress gateway.
- Implement a Queue Drain Worker that meters queued messages back into the service in controlled increments.
- Verify backend stability before transitioning circuit breakers to fully Closed state.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect Istio circuit breaker and outlier detection parameters
kubectl get destinationrules payment-service -o yaml | grep -A 10 outlierDetection

# Manually transition circuit breaker to Half-Open probe state
curl -X POST http://api-gateway:8080/admin/circuits/payment-service/half-open

# Monitor active concurrent requests on payment service
curl -s http://payment-service:8080/metrics | grep active_requests

# Inspect depth of queued deferred requests
redis-cli llen pending_payment_queue

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A recovering service cannot survive an instantaneous wave of queued traffic. I configure circuit breakers with a robust Half-Open state that allows only 5% of traffic through to verify stability. Upstream clients must use exponential backoff with full randomized jitter, and queued transactions are drained through a metered worker rather than dumped all at once."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Enterprise System Design & Disaster Recovery Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Enterprise System Design & Disaster Recovery Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

