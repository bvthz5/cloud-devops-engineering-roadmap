# 02 - Log Analytics Workspace & Kusto Query Language (KQL) Deep Dive

> **Focus:** Big-data log analysis and querying in Microsoft Azure. Deep dive into Log Analytics Workspace architecture, table design, data tiers (Analytics vs. Basic vs. Archive), Kusto Query Language (KQL) operators, performance tuning, and essential production queries.

---

## 🔍 1. What, Why, and History of Log Analytics & KQL

### What is Log Analytics & KQL?
- **Log Analytics Workspace:** A dedicated management container and columnar data store in Azure where telemetry logs are aggregated, indexed, and retained.
- **Kusto Query Language (KQL):** A high-performance, read-only analytical query language developed by Microsoft specifically for querying massive volumes of semi-structured and structured log data.

### Why does Azure Use KQL Instead of SQL?
1. **Pipelined Dataflow Syntax (`|`):** KQL reads left-to-right, top-to-bottom using the pipe operator (`|`). Each operator takes the output table from the previous line and transforms it, making complex queries intuitive and readable compared to deeply nested SQL subqueries.
2. **Columnar Compression & Speed:** KQL queries execute against the Azure Data Explorer engine, scanning billions of records per second with 10x columnar data compression.
3. **Built-in Time-Series & Anomaly Detection:** Native operators for time binning (`bin()`), regex extraction, JSON parsing, and machine-learning clustering.

### 📜 History & Evolution:
- **2014:** Created internally at Microsoft under code name **"Kusto"** (named after French ocean explorer Jacques Cousteau) to explore massive oceans of telemetry data.
- **2018:** KQL standardized across Azure Monitor, Application Insights, and Azure Log Analytics.
- **2019–Present:** Adopted as the universal query foundation for **Microsoft Sentinel (SIEM)** and **Microsoft Defender XDR**.

---

## 🏗️ 2. Anatomy of a KQL Query Pipeline

A KQL query starts with a table name, followed by sequential pipe (`|`) transformations:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              KQL PIPELINED EXECUTION MODEL                             │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ TableName                         <-- 1. Specify the source table                      │
│ | where TimeGenerated > ago(24h)  <-- 2. Filter time range first (Partition Pruning!)  │
│ | where Level == "Error"          <-- 3. Filter specific criteria                      │
│ | extend StatusCode = toint(...)  <-- 4. Calculate or parse JSON attributes            │
│ | summarize count() by bin(TimeGenerated, 1h), StatusCode <-- 5. Aggregate metrics     │
│ | render timechart                <-- 6. Render visual chart in Azure Portal           │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ 3. Essential KQL Operators Reference Table

| KQL Operator | Purpose / Function | SQL Equivalent | Example Usage |
| :--- | :--- | :--- | :--- |
| **`where`** | Filters rows based on boolean conditions | `WHERE` | `| where ResultType == "500"` |
| **`project`** | Selects and renames specific columns | `SELECT col1, col2` | `| project TimeGenerated, OperationName` |
| **`project-away`**| Excludes specific noisy columns | *None* | `| project-away RawData, TenantId` |
| **`extend`** | Creates calculated or parsed columns | `SELECT expr AS newCol` | `| extend DurationMs = Duration / 1000` |
| **`summarize`** | Aggregates data (count, sum, avg) by groups | `GROUP BY` with aggregate | `| summarize count() by bin(TimeGenerated, 1h)` |
| **`take` / `limit`**| Returns first N records | `LIMIT N` / `TOP N` | `| take 50` |
| **`sort by`** | Orders records ascending or descending | `ORDER BY` | `| sort by TimeGenerated desc` |
| **`parse_json()`** | Extracts properties from JSON strings | `JSON_VALUE` | `| extend Caller = parse_json(Properties).caller`|

---

## 📋 4. Production KQL Queries for Troubleshooting

### Query 1: Detect Who Made Changes to Azure RBAC Role Assignments (Audit):
```kusto
AzureActivity
| where TimeGenerated > ago(7d)
| where OperationNameValue has "Microsoft.Authorization/roleAssignments/write"
| where ActivityStatusValue == "Success"
| project TimeGenerated, Caller, CallerIpAddress, ResourceGroup, SubscriptionId, Properties
| order by TimeGenerated desc
```

### Query 2: Find Top 5 Virtual Machines by CPU Utilization Over Last 24 Hours:
```kusto
Perf
| where TimeGenerated > ago(24h)
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where InstanceName == "_Total"
| summarize AvgCPU = round(avg(CounterValue), 2) by Computer
| top 5 by AvgCPU desc
```

### Query 3: Identify Failing HTTP 5xx Endpoints in Application Insights:
```kusto
requests
| where TimeGenerated > ago(4h)
| where resultCode startswith "5"
| summarize FailureCount = count(), AvgDurationMs = round(avg(duration), 1) by name, resultCode
| order by FailureCount desc
```

---

## 💾 5. Data Tiers: Analytics vs. Basic vs. Archive Logs

To dramatically cut log ingestion bills, Azure Monitor offers 3 table tiers:
1. **Analytics Logs (Default):** Full KQL query capability, alerting, joins, dashboards. Retention up to 730 days. Cost: Standard ingestion fee.
2. **Basic Logs:** Ideal for high-volume debug logs. Reduced ingestion cost (up to 80% cheaper). Limited KQL search queries; no cross-table joins.
3. **Archive Logs:** Cold storage for regulatory data (HIPAA/SOC2) up to **12 years (4,383 days)** at a fraction of a cent per GB-month.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure Monitor Architecture Metrics and Logs](./01-Azure-Monitor-Architecture-Metrics-and-Logs.md) | [Index](../../../README.md) | [03 - Application Insights APM and Distributed Tracing →](./03-Application-Insights-APM-and-Distributed-Tracing.md) |
