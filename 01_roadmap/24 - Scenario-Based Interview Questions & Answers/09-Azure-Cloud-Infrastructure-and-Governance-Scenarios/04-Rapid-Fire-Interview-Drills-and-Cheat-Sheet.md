# Azure Cloud Infrastructure & Governance Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Azure Site Recovery (ASR) Replication Fails Due to High Churn Rate

### 🚨 The Production Scenario
Critical production VMs replicated via Azure Site Recovery (ASR) to a secondary Azure region enter 'Critical' health status with replication lag exceeding 12 hours and warnings of 'Data churn rate exceeded supported limits'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The source VMs had tempdb, log files, or streaming cache files located on the OS disk or data disks configured for replication, generating write churn higher than ASR's disk replication limit (e.g., >54 MB/s per disk on Premium SSD).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Identify the disks and processes generating high write I/O on the source VM.
- Move tempdb, pagefile.sys, and non-critical logging directories to temporary or non-replicated scratch disks.
- Configure ASR disk exclusion to replicate only OS and stateful data disks.
- Upgrade Standard SSD replication disks to Premium SSD or High Churn replication policies.
- Trigger ASR resynchronization and verify replication health status returns to 'Normal'.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect ASR recovery vaults and fabrics
az site-recovery fabric list -g rg-recovery -o table

# Check replication health and lag
az site-recovery protected-item show -g rg-recovery --vault-name rsv-prod --name vm-db01 --query 'properties.replicationHealth'

# Identify disk write churn rate on source VM
az monitor metrics list --resource <VM_RESOURCE_ID> --metric 'DiskWriteBytes' --interval PT1M

# Inspect disk drive allocations inside Windows VM
wmic logicaldisk get caption,description,freespace,size

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "ASR replication failure due to high churn is almost always caused by replicating ephemeral data like tempdb, swap files, or verbose log directories. I identify high-write volumes, move tempdb and pagefile to Azure ephemeral local SSDs (D: drive), and exclude those disks from the ASR replication configuration, bringing churn rates well within supported replication thresholds."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Azure Kubernetes Service (AKS) Pod IP Exhaustion with Azure CNI** | `az network vnet subnet show -g rg-prod-network -n snet-aks --vnet-name vnet-prod --query 'ipConfigurations'` | Standard Azure CNI allocates a static pool of IP addresses from the host VNet su... |
| **Scenario 2: Azure Key Vault Access Denied (403 Forbidden) from VM via Managed Identity** | `az vm identity show -g rg-app -n vm-worker --query 'principalId' -o tsv` | Azure Key Vault supports two authorization models: Vault Access Policies and Azu... |
| **Scenario 3: ExpressRoute Circuit Redundancy Drops - BGP Asymmetric Routing Outage** | `az network express-route get-stats -g rg-network -n er-primary` | Asymmetric routing occurred because on-premises routers were advertising routes ... |
| **Scenario 4: Azure DevOps Self-Hosted Agent Scales Out But Fails Job Execution Due to Missing VNet Peering** | `az network vnet peering list -g rg-cicd --vnet-name vnet-agents -o table` | The VMSS agent pool was deployed into an isolated subnet whose VNet lacked VNet ... |
| **Scenario 5: Azure Cosmos DB Request Unit (RU/s) Throttling (HTTP 429) Spikes** | `az cosmosdb sql container throughput show -g rg-data -a cosmos-prod -d appdb -n transactions` | The Cosmos DB container partition key was poorly designed (e.g., using 'tenant_i... |
| **Scenario 6: Azure Policy Blocks Deployment of Terraform Infrastructure via CI/CD** | `az policy assignment show --name 'require-storage-tls12' --scope '/subscriptions/12345678-abcd-1234-abcd-1234567890ab'` | Enterprise Azure Policy assigned at the Management Group scope enforces governan... |
| **Scenario 7: Azure Virtual WAN Hub Router Reaches Throughput Limit During Database Backup Sync** | `az network vhub show -g rg-vwan -n vhub-eastus --query '{ScaleUnits:routingConfiguration, Provisioning:provisioningState}'` | The Virtual WAN Hub router was provisioned with standard scale units (2 scale un... |
| **Scenario 8: Azure Front Door WAF Blocks Legitimate Customer API Payloads** | `az network front-door waf-policy log-analytics-query --query 'AzureDiagnostics \| where Category == "FrontdoorWebApplicationFirewallLog" \| where action_s == "Block" \| project TimeGenerated, ruleName_s, clientIP_s, requestUri_s, details_matches_s'` | Azure Front Door WAF Core Rule Set detected patterns matching SQL Injection (e.g... |
| **Scenario 9: Azure SQL Database Long Transaction Log Fill-Up (Error 9002)** | `SELECT name, log_reuse_wait_desc FROM sys.databases WHERE name = DB_NAME();` | An uncommitted transaction in a long-running batch job prevented the SQL Server ... |
| **Scenario 10: Azure Site Recovery (ASR) Replication Fails Due to High Churn Rate** | `az site-recovery fabric list -g rg-recovery -o table` | The source VMs had tempdb, log files, or streaming cache files located on the OS... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Azure Cloud Infrastructure & Governance Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Google Cloud Platform (GCP) Infrastructure Scenarios: Production Incidents & Triage Scenarios →](../10-Google-Cloud-Platform-GCP-Infrastructure-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

