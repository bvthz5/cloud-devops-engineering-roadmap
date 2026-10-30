# Monitoring, Observability & Telemetry Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Prometheus Scraping Slowdown Due to Target Timeout & Slow Endpoints

### 🚨 The Production Scenario
Prometheus scrape duration exceeds scrape interval (scrape_interval: 15s, scrape_duration_seconds: 28s). Prometheus skips scrape cycles, creating gaps in Grafana dashboards and triggering false-positive alerts.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A custom application `/metrics` endpoint performed synchronous database queries and external network calls inside the metric generation handler, causing the scrape payload to take 25 seconds to generate and deliver.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Query Prometheus metric `scrape_duration_seconds` to identify the slowest scraping targets across the cluster.
- Inspect the offending application's `/metrics` endpoint: verify if expensive calculations are done synchronously on request.
- Refactor application metric exposition: calculate metrics asynchronously in background worker threads and store results in atomic memory gauges.
- Temporarily increase `scrape_timeout` for that specific job in Prometheus while development fixes the endpoint.
- Audit custom Prometheus client instrumentation across microservices.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Identify top 10 slowest scraping targets
curl -s 'http://prometheus:9090/api/v1/query?query=topk(10,scrape_duration_seconds)' | jq .

# Benchmark application metrics generation latency via curl
time curl -s http://app-backend:8080/metrics > /dev/null

# Inspect application web access logs for metrics endpoint timing
kubectl logs -l app=app-backend -n prod --tail=100 | grep -i '/metrics'

# Verify Prometheus configuration file
promtool check config /etc/prometheus/prometheus.yml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A Prometheus /metrics endpoint must ALWAYS read pre-calculated values from memory; it should never perform I/O, database queries, or lock acquisition when scraped. I locate slow endpoints using 'topk(10, scrape_duration_seconds)', work with developers to move expensive computations into background loops, and ensure metrics endpoints return in under 50 milliseconds."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Prometheus High Cardinality Explosion & TSDB OOM CrashLoop** | `promtool tsdb analyze /prometheus/data` | A developer added the user's raw email address or UUID as a metric label in a hi... |
| **Scenario 2: Distributed Tracing Context Propagation Breakage in Microservices** | `curl -v -H 'traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01' http://api.corp/orders` | An intermediate asynchronous message broker (Kafka/RabbitMQ) or internal HTTP cl... |
| **Scenario 3: Alertmanager Notification Storm - 5,000 PagerDuty Alerts During Network Blip** | `amtool alert list --alertmanager.url=http://alertmanager:9093` | Alertmanager routing trees lacked proper inhibition rules (`inhibit_rules`) and ... |
| **Scenario 4: Grafana Loki Ingestion Rate Exceeded (HTTP 429) & Log Drop During Outage** | `kubectl logs -l app.kubernetes.io/name=loki-distributor -n logging \| grep -i 'rate limit exceeded'` | Loki distributors enforce limits on max ingestion rate per tenant (`ingestion_ra... |
| **Scenario 5: OpenTelemetry Collector Memory Spikes & Dropping Spans Due to Export Backpressure** | `kubectl logs -l app.kubernetes.io/name=opentelemetry-collector -n monitoring \| grep -i 'memory limiter'` | The downstream tracing backend (Jaeger/Tempo) experienced storage latency, slowi... |
| **Scenario 6: Silent Monitoring Black Hole - Dead Man's Snitch Fails to Alert** | `curl -s http://prometheus:9090/api/v1/alerts \| grep -i Watchdog` | The monitoring infrastructure relied exclusively on in-cluster alerting. There w... |
| **Scenario 7: SLO Multi-Window Multi-Burn-Rate Alerting Triggers Correctly on Slow Latency Degradation** | `promtool check rules /etc/prometheus/rules/slo_burn_rates.yml` | The team relied on naive short-term threshold alerts that detect sudden massive ... |
| **Scenario 8: Blackbox Exporter External Probing Detects Edge CDN Routing Loop** | `curl -s 'http://blackbox-exporter:9115/probe?target=https://www.example.com&module=http_2xx'` | An edge DNS update or CDN origin rule pointed to a decommissioned external load ... |
| **Scenario 9: Jaeger Distributed Tracing Ingestion Overload - Sampling Rate Tuning** | `curl -s http://jaeger-collector:14269/metrics \| grep jaeger_collector_spans_dropped_total` | Microservices were configured with 100% constant sampling (`sampler.type=const, ... |
| **Scenario 10: Prometheus Scraping Slowdown Due to Target Timeout & Slow Endpoints** | `curl -s 'http://prometheus:9090/api/v1/query?query=topk(10,scrape_duration_seconds)' \| jq .` | A custom application `/metrics` endpoint performed synchronous database queries ... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Monitoring, Observability & Telemetry Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [DevSecOps, Identity & Secrets Management Scenarios: Production Incidents & Triage Scenarios →](../14-DevSecOps-Identity-and-Secrets-Management-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

