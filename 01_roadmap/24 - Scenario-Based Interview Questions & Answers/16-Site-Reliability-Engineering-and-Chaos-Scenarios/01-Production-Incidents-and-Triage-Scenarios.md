# Site Reliability Engineering (SRE) & Chaos Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Production Incident Command - Sev-1 Outage Escalation & Triage Execution

### 🚨 The Production Scenario
The primary payment processing gateway goes down completely on Black Friday, costing $50,000 per minute. Over 40 engineers join a chaotic Zoom bridge, arguing over theories while executives demand continuous status updates.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Absence of structured Incident Command System (ICS). Lack of designated roles (Incident Commander, Tech Lead, Communications Lead) allowed panic and parallel uncoordinated troubleshooting to prolong Mean Time to Resolution (MTTR).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Establish an Incident Commander (IC) who takes absolute operational control and silences chaotic cross-talk.
- Assign distinct ICS roles: Scribe (logs timeline), Communications Lead (updates executives and status page every 15m), and Operations Lead (drives technical investigation).
- Establish a clear hypothesis-testing cadence: only one diagnostic action or rollback executed at a time.
- Isolate the blast radius: immediately revert the last production deployment or fail over to secondary payment provider.
- Conduct a Blameless Post-Mortem within 48 hours to document root cause, timeline, and corrective engineering actions.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Audit most recent commits deployed prior to outage
git log -n 5 --oneline

# Execute immediate rollback of payment service
kubectl rollout undo deployment payment-gateway -n prod

# Publish transparent status page incident update
curl -X POST https://api.statuspage.io/v1/pages/xxx/incidents -H 'Authorization: OAuth xxx' -d 'incident[name]=Payment Gateway Interruption'

# Failover DNS to secondary payment provider
aws route53 change-resource-record-sets --hosted-zone-id Z123 --change-batch file://failover-payment.json

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "During a Sev-1 outage, technical skill is useless without strict Incident Command. As Incident Commander, I immediately quiet the bridge, appoint a Communications Lead to handle stakeholders, and focus the engineering team on containment before root-cause analysis. If containment requires rolling back the latest release or failing over to our secondary provider, we execute that in under 3 minutes."

---

## 📌 Scenario 2: Error Budget Exhaustion - Freezing Feature Releases to Pay Technical Debt

### 🚨 The Production Scenario
The Core Banking Service exhausted 100% of its quarterly error budget in the first 30 days due to repeated buggy feature releases. Product management insists on pushing new features anyway, risking customer SLA breach penalties.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Engineering and Product organizations lacked an agreed-upon Error Budget Policy with enforceable operational consequences when reliability targets are breached.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Review SLO and Error Budget burn report in Prometheus/Grafana.
- Enforce the corporate Error Budget Policy: feature releases are officially frozen for the service until reliability recovers.
- 100% of sprint capacity is redirected to reliability engineering, automated integration testing, observability, and tech debt remediation.
- Establish an executive exception review process requiring VP-level sign-off for emergency regulatory features.
- Re-enable standard release cadence only when the rolling 30-day error budget recovers above the threshold.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Query remaining error budget ratio
curl -s 'http://prometheus:9090/api/v1/query?query=slo:error_budget:remaining_ratio{service="banking"}'

# Audit recent release tags
git-tag -l --sort=-creatordate | head -n 5

# Verify SLO definitions and recording rules
kubectl get prometheusrules -A | grep slo

# Draft release held in quarantine pending error budget recovery
gh release create v2.4.0 --draft

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "An SLO without an enforced Error Budget Policy is just a meaningless number. When our error budget hits zero, our policy mandates an immediate feature release freeze. Product and engineering agree upfront: when reliable, we ship features; when the budget is burned, 100% of sprint effort pivots to reliability, automated testing, and infrastructure stabilization."

---

## 📌 Scenario 3: Chaos Engineering Drill - Simulating AZ Failure Triggers Split-Brain in StatefulSet

### 🚨 The Production Scenario
During a scheduled Chaos Mesh / LitmusChaos experiment simulating an AWS Availability Zone (AZ) network partition, a stateful MongoDB replica set running across 3 AZs enters a split-brain state, with two nodes claiming primary status and corrupting transactions.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The MongoDB replica set was deployed with an even number of voting members (or lacked a dedicated arbiter), and the network partition isolated one node that failed to recognize majority consensus loss before accepting local writes.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Halt the chaos experiment immediately to allow network partition reconciliation.
- Inspect MongoDB replica set status (`rs.status()`) to determine primary and voting member topology.
- Reconfigure the replica set with an odd number of voting members (e.g., 3 or 5) distributed across 3 distinct AZs.
- Configure MongoDB write concern to `w: "majority"` to guarantee that no write is acknowledged unless committed by a majority of voting nodes.
- Re-run the chaos drill to validate clean primary election without data corruption.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect running LitmusChaos experiment status
kubectl get chaosengine -A

# Immediately terminate Chaos Mesh network partition experiment
kubectl delete networkchaos partition-az-a -n chaos

# Inspect MongoDB replica set member states, health, and election terms
mongo --eval 'rs.status()'

# Verify voting topology and quorum distribution
mongo --eval 'rs.conf().members'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Chaos engineering is specifically designed to expose quorum flaws before production strikes. When our drill revealed a split-brain condition, we identified that write concern was not set to majority. By mandating writeConcern: majority and ensuring 3 voting nodes span 3 separate availability zones, a partitioned minority node immediately steps down and rejects writes, preventing data corruption."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Service Mesh & Microservices Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../15-Service-Mesh-and-Microservices-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Site Reliability Engineering (SRE) & Chaos Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

