# 03 - Azure Cost Optimization: Reservations, Savings Plans, Spot VMs & Hybrid Benefit

> **Focus:** Rate optimization and compute cost reduction in Microsoft Azure. Deep dive into Azure Reserved Instances (RIs), Azure Savings Plans for Compute, Azure Spot Virtual Machines (up to 90% discount), and Azure Hybrid Benefit (AHB) license mobility.

---

## 📉 1. What, Why, and History of Azure Rate Optimization

### What is Rate Optimization?
Rate optimization is the FinOps practice of reducing the hourly unit cost of cloud resources without changing the underlying architecture or shutting down workloads, achieved through financial commitments, license mobility, and opportunistic excess capacity.

### Why Must Steady-State Production Workloads Never Run on Pure Pay-As-You-Go?
Pay-As-You-Go (PAYG) provides maximum elasticity (spin up for 10 minutes, shut down, pay only for 10 minutes). However:
1. **The Steady-State Tax:** Core production infrastructure (Kubernetes worker nodes, active database instances, firewalls) runs 24 hours a day, 365 days a year. Paying PAYG for steady-state workloads wastes between 40% and 72% of your budget.
2. **License Double-Diasing:** Enterprises that already own Windows Server and SQL Server licenses with active Software Assurance (SA) can apply those existing licenses to cloud VMs, eliminating the OS license portion of the Azure bill.

---

## ⚖️ 2. Architectural Comparison: Reservations vs. Savings Plans vs. Spot VMs

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE COMPUTE PRICING OPTIONS                             │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. PAY-AS-YOU-GO (PAYG):                                                               │
│    • 100% Elastic, 0% Commitment. Maximum flexibility, Highest hourly cost.            │
│                                                                                        │
│ 2. AZURE SAVINGS PLANS FOR COMPUTE (1-Year or 3-Year Commitment):                      │
│    • Dollar-per-hour spend commitment (e.g., "$20/hour across all compute").           │
│    • Highly Flexible: Applies dynamically across VMs, Container Apps, App Service,     │
│      and Functions regardless of region or OS. Discount: Up to 65%.                   │
│                                                                                        │
│ 3. AZURE RESERVED INSTANCES (1-Year or 3-Year Commitment):                             │
│    • Commitment to a specific VM Family and Region (e.g., D4s_v5 in East US).          │
│    • Deepest Discount: Up to 72% savings compared to PAYG.                             │
│                                                                                        │
│ 4. AZURE SPOT VIRTUAL MACHINES (Opportunistic Spare Capacity):                          │
│    • Microsoft leases unallocated physical datacenter compute capacity.                │
│    • Extreme Discount: Up to 90% savings!                                              │
│    • Caveat: EVICTION WITH 30-SECOND NOTICE when Azure needs the physical capacity back!│
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Feature / Criteria | Azure Reserved Instances (RI) | Azure Savings Plans for Compute | Azure Spot VMs |
| :--- | :--- | :--- | :--- |
| **Commitment Type** | Specific VM Series & Region | Hourly dollar amount ($/hr) | **Zero Commitment** |
| **Discount Level** | **Deepest (Up to 72%)** | High (Up to 65%) | **Maximum (Up to 90%)** |
| **Flexibility Across Services**| Restricted to specified series | **Applies across VMs, AKS, App Service**| Virtual Machines & VMSS only |
| **Workload Suitability** | Predictable, 24/7 static workloads | Changing workloads & multi-region | Batch processing, stateless CI runners |
| **Eviction Risk** | None (Guaranteed allocation) | None (Guaranteed allocation) | **Eviction on 30s notice!** |

---

## 🏷️ 3. Azure Hybrid Benefit (AHB): Slashing License Surcharges

When you provision a Windows Server VM in Azure on standard PAYG, approximately **45% of the hourly cost is the Windows Server OS license surcharge**.

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE HYBRID BENEFIT MECHANICS                             │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│ PAY-AS-YOU-GO WINDOWS VM:                                                               │
│   [ Underlying Linux/Compute Hardware Cost ] + [ Windows OS License Surcharge: +45% ]   │
│                                                                                         │
│ WITH AZURE HYBRID BENEFIT APPLIED:                                                      │
│   [ Underlying Linux/Compute Hardware Cost ] + [ $0.00 (Existing Software Assurance) ]  │
│   ➔ Instantly cuts Windows VM bill by nearly half!                                      │
│                                                                                         │
│ SQL SERVER HYBRID BENEFIT:                                                              │
│   • 1 SQL Server Enterprise Core license with SA covers 4 vCores of Azure SQL Database! │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical CLI: Enabling Azure Hybrid Benefit on an Existing VM

```bash
# Enable Azure Hybrid Benefit on an existing running Windows VM
az vm update \
  --resource-group rg-compute-prod \
  --name vm-prod-win01 \
  --license-type Windows_Server

# Verify license type is applied
az vm show \
  --resource-group rg-compute-prod \
  --name vm-prod-win01 \
  --query "licenseType" -o tsv
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Resource Tagging Governance and Cost Allocation](./02-Resource-Tagging-Governance-and-Cost-Allocation.md) | [Index](../../../README.md) | [04 - Azure Advisor Cost Recommendations and Right Sizing →](./04-Azure-Advisor-Cost-Recommendations-and-Right-Sizing.md) |
