# 06 - Azure Monitor Agent (AMA) & Data Collection Rules (DCR) Architecture

> **Focus:** Modern guest OS telemetry collection on Azure, on-premises (Azure Arc), and multi-cloud virtual machines. Deep dive into the unified Azure Monitor Agent (AMA), retirement of legacy MMA/OMS agents, declarative Data Collection Rules (DCR), ingestion-time KQL data transformations, and multi-homing destinations.

---

## 💻 1. What, Why, and History of AMA and DCR

### What is Azure Monitor Agent (AMA) and DCR?
- **Azure Monitor Agent (AMA):** The modern, consolidated telemetry agent running inside Windows and Linux virtual machines that collects OS event logs, syslog, and performance counters.
- **Data Collection Rules (DCR):** Declarative, cloud-managed Azure Resource Manager (ARM) resources that define precisely what data AMA collects, how it filters and transforms it at ingestion time, and where to route it.

### Why did Microsoft Retire the Legacy MMA / OMS Agent?
1. **Agent Fragmentation:** Previously, administrators had to manage up to four competing agents: Log Analytics Agent (MMA/OMS), Azure Diagnostics Extension (WAD/LAD), Telegraf agent, and the Dependency Agent.
2. **Billing Waste (Lack of Source Filtering):** Legacy agents sent entire event logs blindly to Azure, racking up massive ingestion bills for useless informational noise. DCRs allow granular XPath and KQL ingestion filtering so you only pay for what you need.
3. **Passwordless Security:** Legacy agents required hardcoding workspace primary keys. AMA authenticates purely via **Managed Identity** (Azure VMs) or **Azure Arc credentials** (on-premises servers).

### 📜 History & Evolution:
- **2015–2020:** Microsoft Monitoring Agent (MMA / OMS) served as the primary agent for Log Analytics.
- **2021:** Azure Monitor Agent (AMA) and Data Collection Rules (DCR) launched into General Availability.
- **August 31, 2024:** **Legacy Log Analytics Agent (MMA) permanently retired and deprecated.** AMA is the mandatory standard across all cloud and hybrid infrastructures.

---

## ⚖️ 2. Architectural Comparison: Legacy MMA vs. Modern AMA

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AGENT GENERATIONAL ARCHITECTURE                           │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ LEGACY MMA / OMS AGENT (RETIRED):                                                      │
│   • Configured inside the VM locally or via workspace ID + shared secret key.          │
│   • All-or-nothing log collection; zero source filtering.                              │
│   • Separate agents needed for metrics vs. security vs. diagnostics.                   │
│                                                                                        │
│ MODERN AZURE MONITOR AGENT (AMA + DCR):                                                │
│   • Centrally managed via Azure Resource Manager (DCR resources).                      │
│   • Passwordless authentication using System-Assigned Managed Identity.                │
│   • Ingestion-time KQL filtering drops unneeded debug lines BEFORE billing occurs!     │
│   • Multi-homing: Simultaneously routes security to Sentinel and perf to app workspace.│
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Criteria | Legacy MMA Agent (Deprecated) | Modern Azure Monitor Agent (AMA) |
| :--- | :--- | :--- |
| **Authentication** | Static Workspace ID & Secret Key | **Managed Identity / Azure Arc OIDC** |
| **Configuration Model** | Local registry / workspace files | **Declarative ARM Data Collection Rules (DCR)** |
| **Data Filtering** | None (All events ingested) | **Granular XPath & Ingestion-time KQL** |
| **Multi-Homing** | Complex registry configuration | Native multi-destination routing in DCR |
| **Cross-Platform Parity** | Differing features Windows vs Linux | Identical DCR schema across OS families |

---

## 🏗️ 3. Data Collection Rule (DCR) Pipeline Architecture

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                               DCR DATA INGESTION PIPELINE                               │
│                                                                                         │
│   VIRTUAL MACHINE (Guest OS)                             AZURE MONITOR BACKEND          │
│   ┌───────────────────────────────────────────────┐     ┌────────────────────────────┐  │
│   │ Azure Monitor Agent (AMA)                     │     │ DATA COLLECTION RULE (DCR) │  │
│   │ ┌───────────────────────────────────────────┐ │     │                            │  │
│   │ │ Windows Event Log / Linux Syslog          │ │     │ 1. Data Sources:           │  │
│   │ │ Performance Counters                      │ │     │    • Syslog facilities     │  │
│   │ └─────────────────────┬─────────────────────┘ │     │    • XPath: EventID=4625   │  │
│   └───────────────────────┼───────────────────────┘     │                            │  │
│                           │ Pulls configuration from DCR│ 2. Ingestion-time KQL:     │  │
│                           ▼                             │    source                  │  │
│   ┌───────────────────────────────────────────────┐     │    | where Level == "Error"│  │
│   │ Filtered Telemetry Stream (Outbound HTTPS 443)├────>│                            │  │
│   └───────────────────────────────────────────────┘     │ 3. Destinations:           │  │
│                                                         │    • Log Analytics (Prod)  │  │
│                                                         │    • Microsoft Sentinel    │  │
│                                                         │    • Event Hubs (SIEM)     │  │
│                                                         └────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical Implementation: Deploying AMA and a DCR via CLI

```bash
# 1. Enable System-Assigned Managed Identity on Virtual Machine (Prerequisite for AMA)
az vm identity assign \
  --name vm-prod-worker01 \
  --resource-group rg-compute

# 2. Install Azure Monitor Agent extension on the VM
az vm extension set \
  --name AzureMonitorLinuxAgent \
  --publisher Microsoft.Azure.Monitor \
  --vm-name vm-prod-worker01 \
  --resource-group rg-compute \
  --enable-auto-upgrade true

# 3. Associate the Virtual Machine with an existing Data Collection Rule (DCR)
DCR_ID=$(az monitor data-collection rule show -g rg-monitoring -n dcr-prod-linux-syslog --query id -o tsv)

az monitor data-collection rule association create \
  --name "assoc-vm-worker01-dcr" \
  --resource-group rg-compute \
  --resource $(az vm show -g rg-compute -n vm-prod-worker01 --query id -o tsv) \
  --rule-id $DCR_ID
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Workbooks and Dashboards](./05-Workbooks-and-Dashboards.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
