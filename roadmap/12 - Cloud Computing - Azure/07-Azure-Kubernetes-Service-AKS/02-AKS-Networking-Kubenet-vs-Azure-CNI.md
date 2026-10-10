# 02 - AKS Networking: Kubenet vs. Azure CNI vs. Azure CNI Overlay

> **Focus:** Container network interface (CNI) models, IP address allocation, Subnet CIDR exhaustion, and Pod-to-VNet routing.

---

## 🌐 1. Kubenet vs. Azure CNI vs. Azure CNI Overlay

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│       Kubenet (Basic Networking)     │       │     Azure CNI (Advanced Networking)  │
│                                      │       │                                      │
│  • Pods get private internal IPs     │       │  • Every Pod gets a REAL VNet IP     │
│  • Node acts as a NAT gateway        │       │  • Direct routing to databases/VNet  │
│  • Conserves VNet IP addresses       │       │  • Fast, sub-ms network performance  │
│  • Requires routing tables; max 400  │       │  • High risk of Subnet IP exhaustion │
│    nodes per cluster                 │       │    (Requires massive /20 subnets)    │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

### 🚀 Azure CNI Overlay (The Modern Standard):
**Azure CNI Overlay** combines the high performance of Azure CNI with the IP conservation of Kubenet. Nodes receive IP addresses from your VNet, while pods receive private non-overlapping overlay IPs routed automatically by Azure SDN switches.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - AKS Architecture Managed Control Plane and Node Pools](./01-AKS-Architecture-Managed-Control-Plane-and-Node-Pools.md) | [Index](../../../README.md) | [03 - AKS Authentication Entra ID Integration and Workload Identity →](./03-AKS-Authentication-Entra-ID-Integration-and-Workload-Identity.md) |
