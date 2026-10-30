# 04 - Azure Monitor: Alert Rules, Action Groups & Incident Management

> **Focus:** Automated alerting and incident management in Azure Monitor. Deep dive into Metric Alerts, Log Search Alerts, Activity Log Alerts, Severity scales (Sev 0 to Sev 4), Action Groups, Alert Processing Rules (maintenance window suppression), and automated remediation webhooks.

---

## 🚨 1. What, Why, and History of Azure Monitor Alerts

### What are Alert Rules & Action Groups?
- **Alert Rules:** Automated monitoring conditions that evaluate telemetry data (metrics, logs, or activity events) against defined thresholds at regular intervals.
- **Action Groups:** Reusable collections of notification recipients and automated response mechanisms triggered when an alert fires (e.g., sending an SMS, pinging a Slack/Teams channel, or invoking an Azure Function).

### Why do Operations Teams Need Tiered Alerting?
1. **Mean Time to Detection (MTTD) Reduction:** Alerts notify on-call engineers seconds after an anomaly occurs, before outages impact end-user transactions.
2. **Alert Fatigue Prevention:** Distinguishing between a transient CPU spike (Sev 3) and total database failover (Sev 0).
3. **Automated Self-Healing:** Action Groups can trigger Azure Automation Runbooks or Logic Apps to automatically restart unhealthy container instances or clear disk volumes without waking an engineer at 3 AM.

### 📜 History & Evolution:
- **2016:** Classic Alerts only allowed basic metric triggers on single VMs with email notifications.
- **2018:** Introduction of the unified **Alerts experience** across metrics, logs, and activity logs.
- **2021:** Introduction of **Alert Processing Rules** (formerly Action Rules) allowing planned maintenance window suppression and dynamic multi-subscription routing.

---

## ⚖️ 2. The 3 Types of Azure Monitor Alert Rules

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              TYPES OF AZURE MONITOR ALERT RULES                        │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. METRIC ALERTS (Near Real-Time Evaluation)                                           │
│    • Evaluates numerical time-series metrics every 1 minute.                           │
│    • Low latency, stateful (fires when threshold breached, resolves when normalized).  │
│    • Dynamic Thresholds: Uses machine learning to detect anomalies without hard limits.│
│                                                                                        │
│ 2. LOG SEARCH ALERTS (Deep Complex Logic)                                              │
│    • Runs scheduled KQL queries against Log Analytics every 5 to 15 minutes.           │
│    • Detects error patterns, HTTP status codes, multi-table joins, and text anomalies. │
│                                                                                        │
│ 3. ACTIVITY LOG ALERTS (Governance & Security)                                         │
│    • Fires when specific administrative actions occur in the subscription.             │
│    • Examples: "Virtual Machine Deleted", "Storage Key Regenerated", "Security Alert". │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🎯 3. Severity Scale (Sev 0 to Sev 4)

Azure Monitor categorizes alerts into 5 standardized severity levels:

| Severity | Label | Operational Meaning | Target Response Time | Example Incident |
| :---: | :--- | :--- | :---: | :--- |
| **Sev 0** | **Critical** | Complete outage of mission-critical system | $< 5$ minutes | Production database unreachable; 100% 500 errors |
| **Sev 1** | **Error** | Major feature impaired; high customer impact | $< 15$ minutes | Payment processing API latency $> 5000\text{ ms}$ |
| **Sev 2** | **Warning** | Degradation detected; system may fail soon | $< 1$ hour | Disk storage volume reaching 85% capacity |
| **Sev 3** | **Informational**| Noteworthy event requiring operational review | Next business day | Auto-scaling triggered; node pool expanded |
| **Sev 4** | **Verbose** | Diagnostic trace for debugging | No response required| Routine backup job completed successfully |

---

## 🛠️ 4. Action Groups & Notification Channels

An **Action Group** decouples the alert detection from notification delivery:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                               ACTION GROUP CHANNELS                                    │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ • Human Notifications: Email, SMS, Azure Mobile App Push, Automated Voice Call         │
│ • Incident Management: PagerDuty, Opsgenie, ServiceNow (via secure webhook / ITSM)     │
│ • ChatOps Integration: Microsoft Teams, Slack webhook notifications                    │
│ • Automated Remediation: Azure Automation Runbook (PowerShell), Azure Logic App,       │
│   Azure Function, Azure Event Hubs                                                     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 5. Practical CLI: Creating an Action Group & Metric Alert Rule

```bash
# 1. Create an Action Group for DevOps On-Call
az monitor action-group create \
  --name ag-devops-oncall \
  --resource-group rg-monitoring \
  --short-name "DevOpsOnCall" \
  --email-receiver name="Lead SRE" email="sre-leads@contoso.com" use-common-alert-schema true

# 2. Create a Sev 1 Metric Alert: VM CPU > 85% for 5 minutes
az monitor metrics alert create \
  --name alert-vm-high-cpu \
  --resource-group rg-monitoring \
  --scopes $(az vm show -g rg-compute -n vm-prod-app01 --query id -o tsv) \
  --condition "avg Percentage CPU > 85" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --severity 1 \
  --action ag-devops-oncall \
  --description "Fires when production VM CPU exceeds 85% for over 5 minutes"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Application Insights APM and Distributed Tracing](./03-Application-Insights-APM-and-Distributed-Tracing.md) | [Index](../../../README.md) | [05 - Workbooks and Dashboards →](./05-Workbooks-and-Dashboards.md) |
