# 📊 Monitoring, Logging & Observability — Kids-Mind Short Notes

> **Format:** Simple 1-line definitions, real-life analogies, visual architectural flowcharts, PromQL cheat sheets, and tool comparisons.

---

## 🧭 1. The 3 Pillars of Observability Explained Simply

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                     THE 3 PILLARS OF OBSERVABILITY                       │
│                                                                          │
│   ┌────────────────────┐  ┌────────────────────┐  ┌───────────────────┐  │
│   │      METRICS       │  │        LOGS        │  │      TRACES       │  │
│   │    (Prometheus)    │  │    (Loki / ELK)    │  │ (Jaeger / OTel)   │  │
│   │                    │  │                    │  │                   │  │
│   │ • Numeric counts   │  │ • Timestamped text │  │ • Request path    │  │
│   │ • CPU %, req/sec   │  │ • "Error 500: DB"  │  │ • Microservice ms │  │
│   │ • "IS IT BROKEN?"  │  │ • "WHY BROKEN?"    │  │ • "WHERE STUCK?"  │  │
│   └────────────────────┘  └────────────────────┘  └───────────────────┘  │
└──────────────────────────────────────────────────────────────────────────┘
```

| Pillar | 1-Line Definition (Kids Mind) | Real-Life Analogy | Question It Answers |
| :--- | :--- | :--- | :---: |
| **Metrics** | Numeric time-series values measured at regular intervals (CPU, RAM, HTTP request count). | Your car's speedometer, fuel gauge, and engine temperature needle. | **"Is the system healthy or breaking right now?"** |
| **Logs** | Discrete, timestamped text records describing specific events that just happened. | The car engine computer's black box recording "Oil pressure sensor tripped at 14:02:11". | **"Why did it break and what was the root cause?"** |
| **Traces** | A detailed journey map showing the exact path and latency of a single user request through 10 microservices. | A package tracking receipt showing arrival/departure times at every post office hub. | **"Where in the microservice chain is the request stuck?"** |

---

## 🚦 2. The 4 Golden Signals (Google SRE Framework)

Every DevOps engineer must monitor these 4 critical signals on production dashboards:

| Signal | 1-Line Definition (Kids Mind) | Metric Example | Alert Threshold Example |
| :--- | :--- | :--- | :--- |
| **1. Latency** | The time it takes to service a request (split into successful vs failed requests). | Time for `/api/checkout` to respond. | Alert if $p99$ response latency $> 500\text{ ms}$ for 5 mins. |
| **2. Traffic** | A measure of how much demand is being placed on your service. | HTTP requests per second (RPS), network I/O. | Alert on sudden abnormal spike ($+300\%$) or cliff ($-80\%$). |
| **3. Errors** | The rate of requests that fail (explicit 5xx errors or implicit wrong content). | HTTP 500 Internal Server Errors, database timeouts. | Alert if error rate exceeds $> 1\%$ of total traffic. |
| **4. Saturation** | How full your system is; measures the most constrained resource (CPU, RAM, Disk, DB connections). | Linux memory usage, pool connection count. | Alert when memory or disk usage reaches $> 85\%$. |

---

## 🔥 3. Prometheus & PromQL Cheat Sheet

Prometheus is a time-series database that **pulls (scrapes)** metrics via HTTP GET from application endpoints (`/metrics`).

### 📦 The 4 Prometheus Metric Types:
1. **Counter:** A cumulative number that can only increase or reset to zero on restart (e.g., `http_requests_total`).
2. **Gauge:** A single numerical value that can go arbitrarily up and down (e.g., `node_memory_MemAvailable_bytes`, `cpu_usage_percent`).
3. **Histogram:** Samples observations (usually request durations or response sizes) and counts them in configurable buckets (e.g., `http_request_duration_seconds_bucket`).
4. **Summary:** Similar to histogram, but calculates configurable percentiles ($p50, p90, p99$) on the client side.

### ⚡ Top 5 Essential PromQL Queries:
```promql
# 1. Calculate per-second request rate over the last 5 minutes
rate(http_requests_total[5m])

# 2. Filter requests by status code 500 and group by HTTP method
sum by (method) (rate(http_requests_total{status="500"}[5m]))

# 3. Calculate CPU utilization percentage on Linux servers
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# 4. Calculate 99th percentile (p99) latency across all endpoints
histogram_quantile(0.99, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))

# 5. Calculate percentage of free disk space on root filesystem
(node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"}) * 100
```

---

## 📋 4. Centralized Logging: ELK / EFK vs Grafana Loki

```text
┌────────────────────────────────────────────────────────────────────────┐
│                      LOGGING ARCHITECTURE OPTIONS                      │
│                                                                        │
│   ELK / EFK:    [App Logs] ──► [Logstash/Fluentd] ──► [Elasticsearch]  │
│                                                       (Full-text index)│
│                                                                        │
│   GRAFANA LOKI: [App Logs] ──► [Promtail / Alloy] ──► [Grafana Loki]   │
│                                                       (Index labels    │
│                                                        ONLY = 90% cheap│
└────────────────────────────────────────────────────────────────────────┘
```

| Feature | ELK / EFK Stack | Grafana Loki |
| :--- | :--- | :--- |
| **Core Components** | **E**lasticsearch, **L**ogstash / **F**luentd, **K**ibana | **Loki** (storage), **Promtail / Alloy** (collector), **Grafana** (UI) |
| **Indexing Model** | Indexes the **entire text content** of every log line. | Indexes **ONLY labels/metadata** (e.g., `app="auth"`, `env="prod"`). |
| **RAM & Disk Cost** | High memory and disk usage due to massive inverted indices. | **Ultra-low cost!** Uses up to 90% less disk and RAM than Elasticsearch. |
| **Query Language** | Lucene / KQL (Kibana Query Language) | **LogQL** (Identical syntax and feel to Prometheus PromQL). |
| **Correlation** | Requires jumping between different dashboards. | **1-Click correlation:** Jump from a Prometheus alert spike straight into logs! |

---

## 🛰️ 5. Distributed Tracing & OpenTelemetry (OTel)

```text
User Request (Trace ID: `4bf92f3577b34da6a3ce929d0e0e4736`)
  ├── Span 1: [API Gateway] ........................... (120ms)
  │     ├── Span 2: [Auth Service] .................... (25ms)
  │     └── Span 3: [Order Service] ................... (85ms)
  │           └── Span 4: [PostgreSQL Query] .......... (70ms) ⚠️ BOTTLENECK!
```

- **Trace:** Represents the complete end-to-end journey of a single user request as it traverses across multiple microservices.
- **Span:** A single unit of contiguous work within a trace (e.g., executing an HTTP request or running an SQL query); has a name, start time, duration, and metadata tags.
- **OpenTelemetry (OTel):** The vendor-neutral CNCF open standard for instrumenting, generating, collecting, and exporting metrics, logs, and traces.
- **Jaeger & Zipkin:** Popular open-source distributed tracing backends with UIs for visualizing trace timelines and waterfall graphs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Web Servers & Reverse Proxies Short Notes](./11-Web-Servers-and-Reverse-Proxies-Short-Notes.md) | [Index](../README.md) | [DevSecOps & Security Engineering Short Notes →](./13-DevSecOps-and-Security-Engineering-Short-Notes.md) |
