# Monitoring, Observability & Telemetry Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Prometheus High Cardinality Explosion & TSDB OOM CrashLoop

### 🚨 The Production Scenario
Prometheus server memory skyrockets from 8 GB to 64 GB in 30 minutes, triggering OOMKilled crashes. Prometheus is unable to complete WAL replay upon restart, remaining unavailable.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A developer added the user's raw email address or UUID as a metric label in a high-throughput endpoint: `http_requests_total{user_id="123e4567-..."}`. This created millions of unique time series (high cardinality), overwhelming the Prometheus TSDB memory index and inverted index (postings).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Analyze TSDB cardinality using the Prometheus TSDB Status API or promtool.
- Identify the offending metric name and high-cardinality label.
- Configure `metric_relabel_configs` in prometheus.yaml with an action: labeldrop or regex replace to strip the high-cardinality label at scrape time.
- Temporarily increase memory limits or expand swap to allow Prometheus to complete WAL replay and create a new chunk.
- Educate development teams on metric design: dynamic values belong in distributed traces or logs, never in metric labels.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Analyze local TSDB chunks to identify highest cardinality metrics and labels
promtool tsdb analyze /prometheus/data

# Query top 10 metrics by series count via API
curl -s http://prometheus:9090/api/v1/status/tsdb | jq .data.seriesCountByMetricName

# Monitor Prometheus RAM consumption
kubectl top pod -l app.kubernetes.io/name=prometheus -n monitoring

# Reload Prometheus configuration after adding metric_relabel_configs
curl -X POST http://prometheus:9090/-/reload

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "High cardinality is the number one killer of Prometheus. Putting unbounded values like user IDs or order IDs into metric labels causes exponential TSDB time series explosion. I run 'promtool tsdb analyze' to locate the offending label, drop it immediately at scrape time using 'metric_relabel_configs', and restore service. For business-level granular tracking, we route those events to OpenTelemetry traces or ClickHouse/Loki logs."

---

## 📌 Scenario 2: Distributed Tracing Context Propagation Breakage in Microservices

### 🚨 The Production Scenario
In Jaeger / OpenTelemetry, distributed traces for checkout requests display disconnected root spans. Downstream payment and inventory spans appear as orphaned independent traces rather than a single unified trace tree.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
An intermediate asynchronous message broker (Kafka/RabbitMQ) or internal HTTP client did not inject or extract the W3C Trace Context headers (`traceparent`, `tracestate`) into message metadata, severing the distributed trace context.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect HTTP request headers and Kafka message record headers using tcpdump or consumer debug tools.
- Verify OpenTelemetry SDK instrumentation: ensure both 'Inject' (on producer/client) and 'Extract' (on consumer/server) methods are invoked.
- Enforce W3C TraceContext propagator standard globally across all microservice runtimes.
- Use OpenTelemetry Auto-Instrumentation agents where manual code injection is error-prone.
- Validate end-to-end trace assembly in Jaeger or Grafana Tempo UI.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Send synthetic request with explicit W3C traceparent header
curl -v -H 'traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01' http://api.corp/orders

# Inspect raw TCP packets for presence of traceparent header
tcpdump -i eth0 -A 'tcp port 8080 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' | grep -i traceparent

# Verify log records correlate with trace ID
kubectl logs -l app=payment-service -n prod | grep -i 'trace_id'

# Inspect OpenTelemetry Collector pipeline configuration
kubectl get opentelemetrycollector -n monitoring -o yaml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Distributed tracing relies entirely on the W3C Trace Context header 'traceparent' crossing process boundaries. When traces fragment, an intermediary service failed to propagate headers. I inspect network packets with tcpdump to find the break, ensure OpenTelemetry text map propagators are configured on both producer and consumer sides of our message queues, and enforce structured log correlation using the trace ID."

---

## 📌 Scenario 3: Alertmanager Notification Storm - 5,000 PagerDuty Alerts During Network Blip

### 🚨 The Production Scenario
During a 2-minute core switch restart, Alertmanager sends over 5,000 individual PagerDuty alerts for every pod, container, and target being unreachable, overwhelming on-call engineers.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Alertmanager routing trees lacked proper inhibition rules (`inhibit_rules`) and grouping configurations (`group_by`, `group_wait`, `group_interval`). When a root node or switch failed, both parent and thousands of dependent child alerts fired simultaneously.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Configure Alertmanager `inhibit_rules` so that when a high-level alert like `NodeDown` or `ClusterUnreachable` fires, all dependent alerts (`InstanceDown`, `PodNotReady`, `ServiceUnreachable`) are automatically suppressed.
- Tune grouping parameters: group alerts by `cluster`, `namespace`, and `alertname` with a reasonable `group_wait` (30s) and `group_interval` (5m).
- Implement alert routing to separate critical pages from informational Slack notifications.
- Enforce alert quality: require SLO-based alerting (burn rates) over symptom-based pod alerts.
- Verify alert silencing and inhibition rules using `amtool`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# List all active alerts currently firing in Alertmanager
amtool alert list --alertmanager.url=http://alertmanager:9093

# Create emergency silence rule via amtool
amtool silence add alertname=InstanceDown --duration=2h --comment='Silencing downstream pod alerts during maintenance'

# Inspect current Alertmanager routing and inhibition rules
amtool config show --alertmanager.url=http://alertmanager:9093

# Validate Alertmanager YAML configuration syntax
amtool check-config /etc/alertmanager/alertmanager.yml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Alert storms cause cognitive overload and alert fatigue. I prevent them in Alertmanager using two critical mechanisms: first, 'inhibit_rules', which dynamically suppress thousands of micro-alerts if a parent alert like NodeDown is active; second, smart grouping with group_by: ['alertname', 'namespace'], collapsing 500 pod alerts into a single actionable PagerDuty incident."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← GitOps & Continuous Delivery Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../12-GitOps-and-Continuous-Delivery-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Monitoring, Observability & Telemetry Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

