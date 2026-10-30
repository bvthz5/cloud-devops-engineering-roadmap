# Site Reliability Engineering (SRE) & Chaos Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Cascading Failure Recovery - Throttling & Graceful Degradation Under Overload

### 🚨 The Production Scenario
A downstream database slows down by 50%. Upstream microservices keep retrying aggressively without backoff. As requests pile up, thread pools exhaust across all tiers, and the entire platform crashes in a cascading failure.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Lack of circuit breakers, rate limiters, and exponential backoff with jitter. Clients retried failed requests immediately, multiplying incoming traffic by 5x (retry storm) and preventing the database from ever recovering.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Enable load shedding at the API Gateway: drop non-essential traffic (e.g., recommendation engine, analytics) with HTTP 429 / 503.
- Activate circuit breakers to fail fast immediately without hitting the strained database.
- Enforce exponential backoff and randomized jitter on all client retry logic.
- Implement graceful degradation: serve cached, stale data to users rather than hard 500 error pages.
- Gradually ramp traffic back up using concurrency limits once the database stabilizes.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Emergency shed non-critical traffic at API gateway
curl -X POST http://api-gateway:8080/admin/ratelimit/enable -d 'tier=non-critical&rate=0'

# Apply emergency connection throttling in Istio
kubectl patch destinationrule database-service -n prod --type='merge' -p '{"spec":{"trafficPolicy":{"connectionPool":{"http":{"maxRequestsPerConnection":1}}}}}'

# Check active vs waiting connection backlog in PostgreSQL
SELECT count(*), state FROM pg_stat_activity GROUP BY state;

# Verify availability of fallback read cache
redis-cli ping

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "In a cascading failure, aggressive retries act like a self-inflicted DDoS attack. Recovery requires three decisive actions: first, shed load at the gateway by dropping non-critical features; second, trip circuit breakers to let the database drain its queue; third, mandate exponential backoff with full jitter on all clients so traffic returns in an orderly wave rather than a synchronized surge."

---

## 📌 Scenario 5: Post-Mortem Action Item Fatigue - Preventing Recurrent Outages

### 🚨 The Production Scenario
An organization suffers the exact same database failover outage three times in six months. Post-mortem reviews were held after each incident, but the action items were buried in Jira backlog and never implemented.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Post-mortems were treated as administrative checkboxes rather than engineering commitments. Action items lacked assigned owners, SLAs, and executive accountability, allowing technical debt to cause repeat failures.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Establish a strict Post-Mortem Policy: Sev-1 post-mortems must be published within 72 hours.
- Action items must be categorized into: Preventative (eliminate root cause), Detective (alert faster), and Mitigative (reduce MTTR).
- Assign every action item an explicit engineer owner and a mandatory 30-day SLA.
- Track post-mortem action items in weekly executive engineering operations reviews.
- Block new feature development for that team if post-mortem action items breach SLA.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Audit open post-mortem action items across repositories
gh issue list --label 'post-mortem-p0' --state open

# Query outstanding high-priority reliability engineering tickets
jira issue list -q 'project = REL AND status != Done AND priority = Highest'

# Verify commits linked to post-mortem Jira tickets
git log --grep='RCA-' -n 5 --oneline

# Audit recent operational events
kubectl get events -n prod --sort-by='.metadata.creationTimestamp' | tail -n 20

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A post-mortem without accountability is just an expensive bedtime story. I treat post-mortem action items as top-tier engineering commitments. P0 remediation items have a non-negotiable 30-day SLA tracked directly in executive reviews. If a team has overdue post-mortem items, feature deployments are halted until the systemic reliability gap is closed."

---

## 📌 Scenario 6: Alert Fatigue & On-Call Burnout - Slashing 80% of Non-Actionable Alerts

### 🚨 The Production Scenario
An SRE team receives 1,200 PagerDuty pages a week. Over 90% of pages resolve themselves within 3 minutes without engineer action, causing severe sleep deprivation and high engineer turnover.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Alerts were configured on low-level symptoms (e.g., CPU > 80%, disk I/O blip, single pod restart) rather than customer-impacting symptoms (SLO breaches, user error rates, latency spikes).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Conduct an On-Call Review: export PagerDuty incident reports and group alerts by frequency.
- Delete or demote any alert that does not require an immediate, awake human action to Slack/email.
- Increase alert evaluation windows (e.g., change `for: 1m` to `for: 10m` or `15m`) to eliminate transient noise.
- Transition from threshold alerting to SLO Multi-Burn-Rate alerting.
- Mandate that every single pager alert MUST have a link to a verified, actionable runbook.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Count currently active firing alerts in Alertmanager
amtool alert --alertmanager.url=http://alertmanager:9093 | grep -c firing

# Query total PagerDuty incident count for team
curl -s -H 'Authorization: Token token=xxx' https://api.pagerduty.com/incidents/count | jq .

# Validate cleaned-up Prometheus alert rules
promtool check rules /etc/prometheus/rules/alerts.yml

# Review PR pruning non-actionable alert rules
git diff HEAD~1 /etc/prometheus/rules/alerts.yml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "If an alert does not require an engineer to log into a computer and take immediate action, it has no business waking anyone up. I audit the top 10 noisiest alerts, delete alerts for self-healing transient spikes, move informational alerts to Slack, and replace component-level CPU thresholds with user-facing SLO burn rates. We cut alert volume by 80% while catching real incidents faster."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Site Reliability Engineering (SRE) & Chaos Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Site Reliability Engineering (SRE) & Chaos Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

