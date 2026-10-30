# 01 - Azure Monitor Architecture: Metrics, Logs & Core Observability Pillars

> **Focus:** Full-stack observability architecture in Microsoft Azure. Deep dive into the dual foundational data stores: Time-Series Metrics vs. Structured Kusto Logs, ingestion pipelines, Activity Logs, and real-time visualization.

---

## 📊 1. What, Why, and History of Azure Monitor

### What is Azure Monitor?
Azure Monitor is the comprehensive, centralized observability service built natively into the Azure platform. It collects, analyzes, correlates, and acts on telemetry data generated from your cloud, hybrid on-premises, and multi-cloud environments.

### Why do Cloud Platforms Require a Dual Data Store Model?
Telemetry data falls into two fundamentally distinct mathematical categories:
1. **Time-Series Numerical Metrics:** Numerical values measured at regular intervals (e.g., CPU utilization %, network bytes sent, available disk space). They require sub-second query latency and near-zero alert evaluation lag, but do not need long-term complex text indexing.
2. **Structured & Semi-Structured Logs:** Detailed timestamped events, exception stack traces, HTTP access logs, and audit trails. They require powerful analytical query capabilities (Kusto Query Language), text parsing, multi-table joins, and long-term regulatory retention.

### 📜 History & Evolution:
- **2015:** Telemetry was severely fragmented across isolated services: Azure Operational Insights, Operations Management Suite (OMS), Azure Diagnostics, and Classic Alerts.
- **2018:** Microsoft unified all monitoring, log aggregation, and APM capabilities under the single, standardized **Azure Monitor** umbrella.
- **2021–Present:** Migration from legacy agents to the unified **Azure Monitor Agent (AMA)** and declarative **Data Collection Rules (DCR)**.

---

## ⚖️ 2. Architectural Comparison: Metrics vs. Logs

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE MONITOR DUAL-PILLAR MODEL                           │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ TIME-SERIES METRICS DATA STORE                                                         │
│   • Structure: Lightweight numerical time-series values (Timestamp, MetricName, Value) │
│   • Latency: Near real-time (< 1 minute latency).                                      │
│   • Performance: Ultra-fast retrieval; ideal for metric alert triggers & autoscaling.  │
│   • Retention: 93 days standard in platform store.                                     │
│                                                                                        │
│ LOG ANALYTICS (KUSTO) DATA STORE                                                       │
│   • Structure: Structured & semi-structured records stored in column-oriented tables.  │
│   • Latency: Ingestion lag typically 1 to 3 minutes.                                   │
│   • Query Language: Kusto Query Language (KQL) supporting joins, regex, aggregations.  │
│   • Retention: 30 days free (Configurable up to 730 days / 2 years, or 12 yrs archive) │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Criteria | Azure Monitor Metrics | Azure Monitor Logs (Log Analytics) |
| :--- | :--- | :--- |
| **Data Nature** | Numerical time-series metrics | Rich structured events & text traces |
| **Ingestion Latency**| Real-time (< 1 minute) | 1 to 3 minutes typical |
| **Alert Responsiveness**| Sub-minute near-real-time evaluation | 1 to 15 minute scheduled query execution |
| **Query Mechanism** | Azure Portal Metrics Explorer (visual graphs) | **Kusto Query Language (KQL)** |
| **Autoscaling Driver** | Primary signal driver for VMSS / App Service | Cannot directly drive scale events natively |

---

## 🏗️ 3. Full-Stack Telemetry Ingestion Pipeline

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              TELEMETRY DATA FLOW PIPELINE                              │
│                                                                                        │
│   SOURCES OF TELEMETRY DATA                                                            │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ • Applications: Application Insights SDK (Traces, exceptions, page views)      │   │
│   │ • Guest OS: Azure Monitor Agent (AMA) (Syslog, Windows Event Logs, perf counters│   │
│   │ • Azure Resources: Resource Diagnostic Settings (Key Vault audits, NSG flow logs│   │
│   │ • Azure Subscription: Azure Activity Log (Resource creation, RBAC changes)     │   │
│   │ • Microsoft Entra ID: Sign-in logs, audit logs, Conditional Access evaluations │   │
│   └──────────────────────────────────────┬─────────────────────────────────────────┘   │
│                                          │                                             │
│                                          ▼ Ingestion & Routing                         │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ AZURE MONITOR CORE PLATFORM                                                    │   │
│   │ ┌──────────────────────────┐         ┌───────────────────────────────────────┐ │   │
│   │ │ Metrics In-Memory Store  │         │ Log Analytics Workspace (Kusto)       │ │   │
│   │ └────────────┬─────────────┘         └───────────────────┬───────────────────┘ │   │
│   └──────────────┼───────────────────────────────────────────┼─────────────────────┘   │
│                  │                                           │                         │
│                  ▼                                           ▼                         │
│   ACTION & VISUALIZATION ENGINES                                                       │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ • Alerts & Action Groups: Email, SMS, Webhook, PagerDuty, Logic Apps           │   │
│   │ • Autoscale Engine: Horizontally scales VM Scale Sets and App Service Plans    │   │
│   │ • Workbooks & Dashboards: Interactive visual engineering reports               │   │
│   │ • Streaming Export: Azure Event Hubs / SIEM (Microsoft Sentinel, Splunk, Datadog│   │
│   └────────────────────────────────────────────────────────────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical CLI: Exporting Activity Logs to Log Analytics

Every administrative action across your Azure subscription (e.g., who stopped a VM, who deleted a database) is recorded in the **Activity Log**. Best practice is streaming it into a Log Analytics Workspace for long-term audit compliance:

```bash
# 1. Retrieve the Log Analytics Workspace Resource ID
WORKSPACE_ID=$(az monitor log-analytics workspace show -g rg-monitoring -n law-corp-prod --query id -o tsv)

# 2. Create Diagnostic Setting on Subscription level to stream Activity Logs
az monitor diagnostic-settings subscription create \
  --name "stream-subscription-activity-logs" \
  --location eastus \
  --workspace $WORKSPACE_ID \
  --logs '[{"category": "Administrative", "enabled": true}, {"category": "Security", "enabled": true}, {"category": "Alert", "enabled": true}]'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Module Overview](./00-Overview.md) | [Index](../../../README.md) | [02 - Log Analytics Workspace and Kusto Query Language KQL →](./02-Log-Analytics-Workspace-and-Kusto-Query-Language-KQL.md) |
