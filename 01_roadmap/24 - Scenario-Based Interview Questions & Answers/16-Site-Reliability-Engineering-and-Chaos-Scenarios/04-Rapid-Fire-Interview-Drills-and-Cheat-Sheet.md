# Site Reliability Engineering (SRE) & Chaos Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: SRE Golden Signals Dashboard Anomaly Detection on Checkout Conversion Rate

### 🚨 The Production Scenario
Technical infrastructure metrics (CPU, Memory, Disk, Network) are all 100% green and normal. However, the business analytics dashboard shows that checkout conversion rates dropped by 95% over the last hour.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A third-party address verification API changed its response schema slightly. The frontend silently swallowed the parsing error, preventing the 'Complete Purchase' button from firing while logging zero 500 errors to the backend.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Incorporate Business Metrics (Checkout Completions, Revenue/min, Signups) as primary Golden Signals alongside Latency, Traffic, Errors, and Saturation.
- Implement synthetic browser testing (e.g., Playwright / Datadog Synthetics) executing end-to-end checkout transactions every 60 seconds.
- Inspect client-side error telemetry using Sentry or Datadog RUM (Real User Monitoring).
- Identify the frontend JavaScript unhandled promise rejection and deploy an emergency fix.
- Create Prometheus alerting rules on `rate(checkout_completions_total[5m]) < historical_average * 0.2`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Run automated browser synthetic test of full checkout user journey
npx playwright test tests/e2e-checkout.spec.ts

# Query real-time successful checkout completion rate
curl -s http://prometheus:9090/api/v1/query?query='rate(successful_checkouts_total[5m])'

# Query unhandled client-side exceptions from Sentry
sentry-cli issues list --project web-storefront --status unresolved

# Check deployment status
kubectl get deployments -n prod

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Infrastructure metrics can be 100% green while your business is 100% dead. SREs must monitor Business Key Performance Indicators as first-class Golden Signals. We alert on deviations in real-time checkout conversion rates compared to historical seasonal averages. Combined with continuous synthetic headless browser tests running every minute, we catch silent client-side business failures instantly."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Production Incident Command - Sev-1 Outage Escalation & Triage Execution** | `git log -n 5 --oneline` | Absence of structured Incident Command System (ICS). Lack of designated roles (I... |
| **Scenario 2: Error Budget Exhaustion - Freezing Feature Releases to Pay Technical Debt** | `curl -s 'http://prometheus:9090/api/v1/query?query=slo:error_budget:remaining_ratio{service="banking"}'` | Engineering and Product organizations lacked an agreed-upon Error Budget Policy ... |
| **Scenario 3: Chaos Engineering Drill - Simulating AZ Failure Triggers Split-Brain in StatefulSet** | `kubectl get chaosengine -A` | The MongoDB replica set was deployed with an even number of voting members (or l... |
| **Scenario 4: Cascading Failure Recovery - Throttling & Graceful Degradation Under Overload** | `curl -X POST http://api-gateway:8080/admin/ratelimit/enable -d 'tier=non-critical&rate=0'` | Lack of circuit breakers, rate limiters, and exponential backoff with jitter. Cl... |
| **Scenario 5: Post-Mortem Action Item Fatigue - Preventing Recurrent Outages** | `gh issue list --label 'post-mortem-p0' --state open` | Post-mortems were treated as administrative checkboxes rather than engineering c... |
| **Scenario 6: Alert Fatigue & On-Call Burnout - Slashing 80% of Non-Actionable Alerts** | `amtool alert --alertmanager.url=http://alertmanager:9093 \| grep -c firing` | Alerts were configured on low-level symptoms (e.g., CPU > 80%, disk I/O blip, si... |
| **Scenario 7: SRE Capacity Planning - Modeling Black Friday 10x Load Spikes** | `k6 run --vus 5000 --duration 30m load-test-script.js` | Capacity planning was done through guesswork rather than synthetic load testing,... |
| **Scenario 8: Thundering Herd Problem - Cache Invalidation Triggers Complete Database Death** | `redis-cli set product_catalog_lock 1 NX EX 10` | The 'Thundering Herd' (cache stampede) effect: multiple concurrent requests expe... |
| **Scenario 9: GameDay Disaster Recovery - Live Data Center Cutover Drill** | `dig www.example.com +nostats +nocmd` | The DNS A/CNAME records had a Time-To-Live (TTL) set to 86,400 seconds (24 hours... |
| **Scenario 10: SRE Golden Signals Dashboard Anomaly Detection on Checkout Conversion Rate** | `npx playwright test tests/e2e-checkout.spec.ts` | A third-party address verification API changed its response schema slightly. The... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Site Reliability Engineering (SRE) & Chaos Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Database Reliability Engineering (DBRE) Scenarios: Production Incidents & Triage Scenarios →](../17-Database-Reliability-Engineering-DBRE-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

