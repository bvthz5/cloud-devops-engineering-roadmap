# 04 - Virtual Machine Scale Sets (VMSS) & Autoscaling

> **Focus:** Horizontal elasticity, VMSS architecture, metric-driven auto-scaling, cooldown duration, Uniform vs. Flexible orchestration modes, and health probe integration.

---

## 📈 1. How VM Scale Sets Work

A **Virtual Machine Scale Set (VMSS)** is an Azure compute management abstraction that deploys and manages a group of load-balanced, identical virtual machines.

```text
                      User Traffic Surges
                              │
                              ▼
           Azure Monitor Metric: Average CPU > 70%
                              │
                              ▼
            Scale-Out Rule Triggers (+2 VMs)
    ┌──────────────────────────────────────────────────┐
    │              VM Scale Set (VMSS)                 │
    │  [VM-1]  [VM-2]  ◄── [VM-3]  [VM-4] Added        │
    └──────────────────────────────────────────────────┘
                              │
              Cooldown Period (e.g., 5 Minutes)
            (Prevents rapid fluctuation / thrashing)
                              │
                              ▼
           Traffic Normalizes: Average CPU < 30%
                              │
                              ▼
             Scale-In Rule Triggers (-1 VM)
```

---

## ⚙️ 2. Core Autoscaling Parameters

| Parameter | Function | Production Recommendation |
| :--- | :--- | :--- |
| **Minimum Instances** | The baseline floor of instances always running. | At least `2` across different AZs for high availability. |
| **Maximum Instances** | The ceiling preventing unbounded financial cost spikes. | Set to match database connection pool maximums (e.g., `10` or `20`). |
| **Scale-Out Rule** | Condition to add VM instances (e.g., CPU > 75% for 5 min). | Add instances in increments of 1 or 2 based on warmup time. |
| **Scale-In Rule** | Condition to terminate VMs when demand drops. | Remove 1 instance when CPU drops below 25% for 10 min. |
| **Cooldown Period** | Time to wait after a scale event before evaluating metrics again. | `5` to `10` minutes (Allows new VMs to boot and take traffic). |

---

### 💻 Production Azure CLI VMSS with Autoscaling

```bash
# 1. Create a VM Scale Set with 2 initial instances
az vmss create   --resource-group demo-rg   --name my-scaleset   --image Ubuntu2204   --vm-sku Standard_B2s   --instance-count 2   --admin-username azureuser   --generate-ssh-keys

# 2. Configure Autoscaling Framework (Min 2, Max 10, Default 2)
az monitor autoscale create   --resource-group demo-rg   --resource my-scaleset   --resource-type Microsoft.Compute/virtualMachineScaleSets   --name autoscale-rule   --min-count 2   --max-count 10   --count 2

# 3. Add Scale-Out Rule: Add 2 VMs if Average CPU > 70% over 5 minutes
az monitor autoscale rule create   --resource-group demo-rg   --autoscale-name autoscale-rule   --condition "Percentage CPU > 70 avg 5m"   --scale out 2

# 4. Add Scale-In Rule: Remove 1 VM if Average CPU < 30% over 5 minutes
az monitor autoscale rule create   --resource-group demo-rg   --autoscale-name autoscale-rule   --condition "Percentage CPU < 30 avg 5m"   --scale in 1
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - High Availability Availability Sets vs Availability Zones](./03-High-Availability-Availability-Sets-vs-Availability-Zones.md) | [Index](../../../README.md) | [05 - Azure Load Balancer Standard vs Basic →](./05-Azure-Load-Balancer-Standard-vs-Basic.md) |
