# 03 - Application Insights: APM, Distributed Tracing & OpenTelemetry

> **Focus:** Application Performance Monitoring (APM) in Azure. Deep dive into Application Insights architecture, W3C Distributed Tracing, OpenTelemetry SDK integration, Application Map visual dependency graphs, and Live Metrics streaming.

---

## 📈 1. What, Why, and History of Application Insights

### What is Application Insights?
Application Insights is an extensible Application Performance Monitoring (APM) feature of Azure Monitor that instruments web apps and backend services to monitor live performance, track exceptions, profile server requests, and map distributed dependencies across microservice architectures.

### Why do Modern Distributed Systems Require APM?
In a monolithic system, logging an exception with a stack trace was sufficient. In a modern cloud microservices architecture:
1. **Distributed Request Hops:** A single user action (e.g., "Confirm Order") triggers an API gateway, which calls the user service, inventory service, payment service, and notification service across multiple containers and databases.
2. **Finding the Latency Culprit:** When a checkout takes 8 seconds instead of 200 ms, Application Insights measures the exact duration spent in every individual SQL query, Redis cache fetch, or third-party HTTP call.
3. **Automated Root-Cause Correlation:** Correlates an uncaught client-side JavaScript error directly to the corresponding backend 500 server exception using a shared correlation ID.

### 📜 History & Evolution:
- **2014:** Launched under the Visual Studio product group as a specialized telemetry agent for .NET and web applications.
- **2020:** **Workspace-based Application Insights** introduced, standardizing all Application Insights telemetry to store inside standard Log Analytics Workspaces.
- **2022–Present:** Microsoft transitions from proprietary Application Insights SDKs to the vendor-neutral **OpenTelemetry (OTel)** industry standard with Azure Monitor OpenTelemetry Exporters.

---

## 🌐 2. W3C Distributed Tracing & Correlation Architecture

Application Insights correlates multi-service requests using the **W3C TraceContext** standard:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              DISTRIBUTED TRACING FLOW (W3C)                            │
│                                                                                        │
│   CLIENT BROWSER (operation_Id: 4bf92f3577b34da6a3ce929d0e0e4736)                      │
│   └──> HTTP POST /api/checkout                                                         │
│        HTTP Header: traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01 │
│                                                                                        │
│   MICROSERVICE A: ORDERS API (Host: AKS Node 1)                                        │
│   ├── Receives request; logs to 'requests' table with operation_Id.                    │
│   └── Makes Outbound HTTP call to Payments API:                                        │
│       ├── Logs to 'dependencies' table (type: HTTP, duration: 420ms)                   │
│       └── Injects updated parent ID into outbound HTTP traceparent header.             │
│                                                                                        │
│   MICROSERVICE B: PAYMENTS API (Host: AKS Node 2)                                      │
│   ├── Receives request; logs to 'requests' table with SAME operation_Id!               │
│   └── Executes SQL query against Azure SQL Database:                                   │
│       └── Logs to 'dependencies' table (type: SQL, target: sql-prod.database...)       │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 🗺️ Application Map:
Application Insights automatically constructs an interactive visual graph showing all service components, their throughput (calls/minute), average latency (ms), and error percentage, highlighting failing dependencies in red.

---

## 📊 3. Core Telemetry Tables in Log Analytics

| Table Name | Description | Key Columns |
| :--- | :--- | :--- |
| **`requests`** | Inbound HTTP/gRPC requests handled by the server | `name`, `url`, `duration`, `resultCode`, `success` |
| **`dependencies`** | Outbound calls made to SQL, HTTP, Redis, Queues | `target`, `type`, `data`, `duration`, `success` |
| **`exceptions`** | Uncaught exceptions, crashes, and stack traces | `problemId`, `type`, `assembly`, `innermostMessage`|
| **`traces`** | Application diagnostic logs (`ILogger`, `console.log`)| `message`, `severityLevel`, `customDimensions` |
| **`pageViews`** | Client-side frontend browser page loads | `name`, `url`, `duration`, `client_CountryOrRegion`|

---

## 💻 4. Code Example: Instrumenting Node.js with OpenTelemetry

```javascript
// index.js - Using modern OpenTelemetry Azure Monitor Distro
const { useAzureMonitor } = require("@azure/monitor-opentelemetry");

// 1. Initialize OpenTelemetry with Azure Monitor connection string
useAzureMonitor({
  connectionString: process.env.APPLICATIONINSIGHTS_CONNECTION_STRING,
  samplingRatio: 1.0 // 100% telemetry capture in staging; adjust in high-traffic prod
});

const express = require("express");
const app = express();

app.get("/api/checkout", async (req, res) => {
  // Application Insights automatically captures incoming request,
  // correlates downstream database calls, and logs any unhandled exceptions!
  res.status(200).json({ status: "Payment successful" });
});

app.listen(3000, () => console.log("Server listening on port 3000"));
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Log Analytics Workspace and Kusto Query Language KQL](./02-Log-Analytics-Workspace-and-Kusto-Query-Language-KQL.md) | [Index](../../../README.md) | [04 - Azure Monitor Alerts and Action Groups →](./04-Azure-Monitor-Alerts-and-Action-Groups.md) |
