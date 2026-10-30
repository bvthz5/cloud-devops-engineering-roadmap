# 05 - Azure Workbooks & Dashboards: Interactive Visual Analytics & NOC Displays

> **Focus:** Visualization and interactive telemetry reporting in Azure Monitor. Deep dive into Azure Workbooks vs. Azure Dashboards vs. Azure Managed Grafana, parameters, cross-resource KQL reporting, and building operational NOC (Network Operations Center) displays.

---

## 📊 1. What, Why, and History of Azure Visualizations

### What are Azure Workbooks and Dashboards?
- **Azure Workbooks:** Interactive, data-driven analytical documents that blend rich Markdown narrative guides, parameters, metrics graphs, KQL log queries, and actionable buttons into a single shareable canvas.
- **Azure Dashboards:** High-level status displays composed of customizable operational tiles pinned from various Azure Portal services, designed for full-screen NOC (Network Operations Center) wall monitors.

### Why did Workbooks Supersede Static Portal Dashboards?
1. **Interactive Parameterization:** Workbooks allow operators to select dynamic dropdown filters (e.g., Subscription, Time Range, Resource Group, VM Name) that instantly re-run all underlying KQL queries across the document.
2. **Troubleshooting Runbooks in Context:** Combines documentation with live live data: an SRE can read step-by-step incident resolution instructions with real-time health graphs embedded directly between the steps.
3. **Cross-Resource & Cross-Tenant Analytics:** A single workbook can query 50 different Log Analytics Workspaces and 200 subscriptions concurrently to generate an organization-wide compliance or cost report.

### 📜 History & Evolution:
- **2014:** Azure Dashboards launched as customizable grid tiles inside the new Ibiza Azure Portal.
- **2019:** **Azure Workbooks** introduced, revolutionizing cloud reporting by bringing Jupyter Notebook-like analytics to Azure Monitor.
- **2022–Present:** Azure Workbooks open-sourced on GitHub; community templates available for AKS, Key Vault, FinOps, and Defender for Cloud.

---

## ⚖️ 2. Architectural Comparison: Workbooks vs. Dashboards vs. Grafana

| Feature / Criteria | Azure Workbooks | Azure Dashboards | Azure Managed Grafana |
| :--- | :--- | :--- | :--- |
| **Primary Purpose** | Deep-dive analytical reports & runbooks | High-level NOC status overview | Real-time multi-cloud metric graphs |
| **Interactivity** | **High** (Dynamic dropdown parameters)| Low (Static pinned tiles) | **High** (Variables, PromQL filters) |
| **Underlying Query Engine**| **KQL (Log Analytics)**, Metrics, Resource Graph | Azure Metrics & pinned views | **PromQL**, Log Analytics, InfluxDB |
| **Documentation Embedded** | Rich GitHub-flavored Markdown supported | Static markdown tiles | Rich markdown & text panels |
| **Cross-Resource Ability** | Queries up to 100+ workspaces at once | Limited to pinned resources | Full cross-data-source dashboards |
| **Best Audience** | SREs, Incident Responders, FinOps Leads | Operations NOC walls, IT Managers | DevOps, Kubernetes, Platform Engineers |

---

## 🏗️ 3. Azure Workbook Architecture & Component Flow

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE WORKBOOK DATA FLOW ARCHITECTURE                     │
│                                                                                        │
│   USER INPUT PARAMETERS (Dropdowns at Top of Workbook)                                 │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ [Subscription: Production-Core]  [TimeRange: Last 24 Hours]  [Environment: Prod]│   │
│   └──────────────────────────────────────┬─────────────────────────────────────────┘   │
│                                          │ Parameters passed down to queries           │
│                                          ▼                                             │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ SECTION 1: SYSTEM HEALTH OVERVIEW (KQL Query)                                  │   │
│   │ • Heartbeat table query: Checks which VMs are online / offline                 │   │
│   │ • Rendered as: Colored Green/Red Status Badges                                 │   │
│   └────────────────────────────────────────────────────────────────────────────────┘   │
│                                          │                                             │
│                                          ▼                                             │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ SECTION 2: CPU & MEMORY PERFORMANCE (Azure Monitor Metrics)                    │   │
│   │ • Multi-line time chart showing % CPU across all VMs in selected resource group │   │
│   │ • Rendered as: Smooth Interactive Time-Series Graph                            │   │
│   └────────────────────────────────────────────────────────────────────────────────┘   │
│                                          │                                             │
│                                          ▼                                             │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ SECTION 3: RECENT 5XX ERROR TRACES (Application Insights KQL)                  │   │
│   │ • Requests table joined with Exceptions table                                  │   │
│   │ • Rendered as: Expandable Data Grid with clickable links to traces            │   │
│   └────────────────────────────────────────────────────────────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical Implementation: Querying via Azure Resource Graph in Workbooks

Workbooks support **Azure Resource Graph (ARG)** queries, enabling lightning-fast inventory visualization across thousands of resources:

```kusto
// Discover untagged virtual machines across all subscriptions
resources
| where type == "microsoft.compute/virtualmachines"
| where isnull(tags.Environment) or isnull(tags.CostCenter)
| project Name = name, ResourceGroup = resourceGroup, Location = location, SubscriptionId = subscriptionId
| order by Name asc
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Azure Monitor Alerts and Action Groups](./04-Azure-Monitor-Alerts-and-Action-Groups.md) | [Index](../../../README.md) | [06 - Azure Monitor Agent AMA and Data Collection Rules DCR →](./06-Azure-Monitor-Agent-AMA-and-Data-Collection-Rules-DCR.md) |
