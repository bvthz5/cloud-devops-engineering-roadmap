# 04 - Azure Advisor: Automated Optimization & Workload Right-Sizing

> **Focus:** Continuous architectural hygiene in Microsoft Azure. Deep dive into Azure Advisor engine internals, the 5 recommendation categories, discovering underutilized compute instances, finding orphaned unattached disks and idle public IPs, and automating right-sizing remediation.

---

## 🧭 1. What, Why, and History of Azure Advisor

### What is Azure Advisor?
Azure Advisor is a personalized, cloud-native automated consulting engine that continuously analyzes your Azure resource telemetry, configurations, and spending patterns against Microsoft architectural best practices to deliver actionable recommendations.

### Why do Operations Teams Rely on Azure Advisor?
1. **Automated Continuous Optimization:** No human engineer can manually audit 1,000 virtual machines and 200 databases every week to verify if they are properly sized. Advisor runs continuously in the background.
2. **Identifying Zombie Resources:** Detects orphaned storage disks, unassociated public IP addresses, and empty load balancers that continue to rack up monthly charges after VMs are deleted.
3. **Low-Risk Performance Right-Sizing:** Uses machine learning models trained on 7 days of historical CPU, memory, and network telemetry to recommend exact target VM sizes that slash bills without risking application performance.

---

## 🏛️ 2. The 5 Azure Advisor Architectural Pillars

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE 5 ADVISOR RECOMMENDATION PILLARS                      │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. COST OPTIMIZATION:                                                                  │
│    • Right-size or shut down underutilized virtual machines (CPU < 5%).                │
│    • Purchase Reserved Instances for steady-state databases and VMs.                   │
│    • Delete unattached Managed Disks and unassociated public IPs.                      │
│                                                                                        │
│ 2. SECURITY:                                                                           │
│    • Powered directly by Microsoft Defender for Cloud recommendations.                 │
│                                                                                        │
│ 3. RELIABILITY (High Availability):                                                    │
│    • Ensure virtual machines are deployed across Availability Zones.                   │
│    • Enable geo-redundant backups on critical storage accounts and SQL databases.      │
│                                                                                        │
│ 4. PERFORMANCE:                                                                        │
│    • Migrate storage accounts to Azure Premium SSD / Ultra Disks.                      │
│    • Optimize database queries, indices, and Cosmos DB request unit (RU/s) limits.     │
│                                                                                        │
│ 5. OPERATIONAL EXCELLENCE:                                                             │
│    • Create Service Health alerts to get notified of upcoming platform maintenance.   │
│    • Ensure Azure Resource Manager deployment templates adhere to best practices.      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🧟 3. Hunting Zombie Resources: Orphaned Disks & Idle IPs

A classic source of cloud waste is deleted VMs leaving behind their attached disks:

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              HOW ORPHANED DISKS GENERATE BILLS                          │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. Engineer deletes VM: az vm delete -n vm-test01.                                      │
│ 2. By default, Azure DELETES THE VM COMPUTE BUT PRESERVES THE MANAGED OS DISK!          │
│ 3. The 128 GB Premium SSD disk sits unattached in the Resource Group.                   │
│ 4. Microsoft bills ~$20/month per disk indefinitely because storage capacity is reserved│
│ 5. Across an enterprise with 50 deleted VMs, that is $1,000/month in 100% WASTED SPEND! │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical CLI: Discovering Cost Recommendations & Orphaned Disks

### 1. Retrieve All Cost Recommendations via Azure CLI:
```bash
az advisor recommendation list \
  --category Cost \
  --query "[].{Impact:impact, Problem:shortDescription.problem, Solution:shortDescription.solution}" \
  --output table
```

### 2. Find and Delete All Unattached "Zombie" Managed Disks:
```bash
# Query all unattached disks using Azure Resource Graph
az graph query -q "resources | where type == 'microsoft.compute/disks' and properties.diskState == 'Unattached' | project name, resourceGroup, properties.diskSizeGB, location" -o table

# Delete an unattached orphaned disk
az disk delete \
  --name "vm-test01_OsDisk_1_d64c" \
  --resource-group "rg-test" \
  --yes
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Azure Reservations and Savings Plans Cost Optimization](./03-Azure-Reservations-and-Savings-Plans-Cost-Optimization.md) | [Index](../../../README.md) | [05 - Azure Well Architected Framework 5 Pillars →](./05-Azure-Well-Architected-Framework-5-Pillars.md) |
