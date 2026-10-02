# 02 - Log Analytics Workspace & KQL Syntax

Kusto Query Language (KQL) for querying log data:
```sql
AppRequests
| where TimeGenerated > ago(1h)
| where Success == false
| summarize FailedCount = count() by OperationName
| order by FailedCount desc
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Azure Monitor Architecture](./01-Azure-Monitor-Architecture-Metrics-and-Logs.md) | [README](./README.md) | [03 - Application Insights APM](./03-Application-Insights-APM-and-Distributed-Tracing.md) |
