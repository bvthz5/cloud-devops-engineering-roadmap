# Monitoring, Observability & Telemetry Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Grafana Loki Ingestion Rate Exceeded (HTTP 429) & Log Drop During Outage

### 🚨 The Production Scenario
During a major production incident, application services print millions of error stack traces. Loki begins rejecting incoming log pushes with HTTP 429 Too Many Requests: 'entry too far behind' or 'ingestion rate limit exceeded'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Loki distributors enforce limits on max ingestion rate per tenant (`ingestion_rate_mb`, `ingestion_burst_size_mb`). The massive log spike exceeded the default 4MB/s limit, causing Promtail / Fluent Bit log shippers to drop logs or buffer to disk.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Increase `ingestion_rate_mb` and `ingestion_burst_size_mb` in Loki limits_config.
- Scale the Loki distributor component horizontally to handle increased network ingest.
- Tune Fluent Bit / Promtail client buffers with backoff retries and local disk buffering.
- Filter high-frequency non-essential log lines at the agent edge using Fluent Bit regex parsers before shipping to Loki.
- Ensure structured metadata and stream labels do not cause label cardinality explosions in Loki chunks.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Search Loki distributor logs for rate limit rejections
kubectl logs -l app.kubernetes.io/name=loki-distributor -n logging | grep -i 'rate limit exceeded'

# Scale distributor tier to absorb log throughput
kubectl scale deployment loki-distributor -n logging --replicas=6

# Measure incoming log bytes rate
curl -s http://loki:3100/metrics | grep loki_distributor_bytes_received_total

# Inspect active log streams and label combinations
logcli series --since=1h '{namespace="prod"}'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Dropping logs during an active outage is disastrous because it blinds engineers when they need visibility most. I increase Loki's ingestion and burst rate limits in limits_config, scale the stateless distributor pods horizontally, and configure Fluent Bit with local disk buffering so no logs are lost if backpressure occurs."

---

## 📌 Scenario 5: OpenTelemetry Collector Memory Spikes & Dropping Spans Due to Export Backpressure

### 🚨 The Production Scenario
OpenTelemetry Collector (Otel-Collector) pods consume 100% of their memory limit and restart frequently. Monitoring reports high `otelcol_exporter_enqueue_failed_spans` metric values.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The downstream tracing backend (Jaeger/Tempo) experienced storage latency, slowing down span ingestion. The Otel Collector's batch processor kept queueing incoming spans in memory until the pod hit its cgroup memory limit and was killed.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Configure the OpenTelemetry Memory Ballast extension or Memory Limiter processor as the first processor in the pipeline.
- Tune the `queued_retry` and `batch` processor settings: limit queue size and enforce dropping policies if memory reaches critical limits.
- Enable persistent queue storage in the Otel Collector using file_storage extension to spool spans to disk during downstream backpressure.
- Scale the Otel Collector horizontally using the OpenTelemetry Operator with Target Allocator.
- Scale the downstream backend storage cluster.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check memory limiter trigger events in Otel Collector
kubectl logs -l app.kubernetes.io/name=opentelemetry-collector -n monitoring | grep -i 'memory limiter'

# Query dropped spans metric from Otel Collector Prometheus exporter
curl -s http://otel-collector:8888/metrics | grep otelcol_exporter_enqueue_failed_spans

# Inspect OpenTelemetry Collector CRD configuration
kubectl get otelcol -n monitoring -o yaml

# Inspect collector memory usage
kubectl top pods -l app.kubernetes.io/name=opentelemetry-collector -n monitoring

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The OpenTelemetry Collector must be protected against backpressure when downstream backends slow down. I mandate the 'memory_limiter' processor as the first step in every pipeline to prevent OOMKilled crashes. For mission-critical telemetry, we configure the file_storage extension for persistent queueing, ensuring spans spool to disk rather than exhausting memory or dropping silently."

---

## 📌 Scenario 6: Silent Monitoring Black Hole - Dead Man's Snitch Fails to Alert

### 🚨 The Production Scenario
An entire Kubernetes cluster network crashed, killing Prometheus, Alertmanager, and all application pods. Because Prometheus was dead, no alerts were sent, and the engineering team only discovered the outage 45 minutes later via customer tweets.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The monitoring infrastructure relied exclusively on in-cluster alerting. There was no external watchdog alert (Dead Man's Switch / Heartbeat) monitoring the health of the monitoring stack itself.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Implement a constant firing alert in Prometheus: `Watchdog` (always-firing alert with severity `none`).
- Route the `Watchdog` alert via Alertmanager to an external heartbeat monitoring service (e.g., Dead Man's Snitch, Healthchecks.io, or PagerDuty Heartbeat).
- The external service expects a ping every 60 seconds; if Prometheus or Alertmanager dies, the external service triggers a PagerDuty Critical incident.
- Deploy multi-cluster redundant Prometheus instances scraping each other across different regions/VPCs.
- Test the watchdog fail-safe quarterly by terminating monitoring pods.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify Watchdog alert is continuously firing in Prometheus
curl -s http://prometheus:9090/api/v1/alerts | grep -i Watchdog

# Verify Alertmanager is routing Watchdog alert
amtool alert list | grep Watchdog

# Test external heartbeat ping endpoint
curl -d '{"status":"firing"}' https://nosnch.in/c2d3e4f5

# Simulate monitoring failure to test external watchdog alerting
kubectl delete pod -l app.kubernetes.io/name=prometheus -n monitoring

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Who monitors the monitor? If your monitoring cluster dies, it cannot alert you to its own death. I always implement the Prometheus 'Watchdog' alert routed to an external SaaS heartbeat like Dead Man's Snitch. It pings the external endpoint every minute. The moment the monitoring stack fails, the external SaaS notices the missing heartbeat and pages on-call within 2 minutes."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Monitoring, Observability & Telemetry Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Monitoring, Observability & Telemetry Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

