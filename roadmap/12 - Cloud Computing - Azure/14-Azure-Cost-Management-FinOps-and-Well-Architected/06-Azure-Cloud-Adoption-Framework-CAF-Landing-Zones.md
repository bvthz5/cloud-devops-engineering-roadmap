# 06 - Cloud Adoption Framework (CAF): Enterprise Landing Zone Architecture

> **Focus:** Enterprise cloud foundations in Microsoft Azure. Deep dive into the Cloud Adoption Framework (CAF), Azure Landing Zone architecture (Enterprise-Scale), Hub-and-Spoke network topology, Azure Virtual WAN, Management Group hierarchy, and subscription vending machines.

---

## 🏛️ 1. What, Why, and History of Azure Landing Zones

### What is an Azure Landing Zone?
An Azure Landing Zone is a multi-subscription, governance-hardened cloud environment that accounts for scale, security governance, networking topology, identity boundaries, and operations tooling to enable organizations to host mission-critical workloads in Azure safely.

### Why do Enterprise Migrations Fail Without Landing Zones?
When organizations migrate to the cloud without a standardized landing zone:
1. **IP Address Collisions:** Development teams provision overlapping CIDR blocks (`10.0.0.0/16`), making cross-VNet peering and hybrid on-premises connectivity impossible later.
2. **Egress Security Blindspots:** Workloads deploy direct public IPs with no centralized egress inspection, opening the network to command-and-control malware exfiltration.
3. **Fragmented Governance:** Every application team invents their own logging, backup, and RBAC schemes, creating catastrophic compliance failures.

### 📜 History & Evolution:
- **2018:** Microsoft publishes the initial **Cloud Adoption Framework (CAF)** whitepapers.
- **2020:** Microsoft launches **Enterprise-Scale Architecture (ESLZ)**, standardizing management group hierarchies and automated ARM/Bicep landing zone accelerators.
- **Current Standard:** **Azure Landing Zones (ALZ)** with automated Subscription Vending Machines via Bicep and Terraform.

---

## 🏗️ 2. The Standard Azure Landing Zone Architecture

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE LANDING ZONE HIERARCHY                              │
│                                                                                        │
│   ROOT MANAGEMENT GROUP (Tenant Root)                                                  │
│   └── ALZ ROOT MANAGEMENT GROUP (Contoso)                                              │
│       │                                                                                │
│       ├── PLATFORM MANAGEMENT GROUP (Core Shared Services)                             │
│       │   ├── Management Subscription: Central Log Analytics & Azure Automation       │
│       │   ├── Connectivity Subscription: Hub VNet, Azure Firewall, ExpressRoute GW    │
│       │   └── Identity Subscription: Active Directory Domain Controllers               │
│       │                                                                                │
│       ├── LANDING ZONES (Workloads)                                                    │
│       │   ├── Corporate (Internal RFC 1918 Private Spoke Subscriptions)                │
│       │   │   ├── Online Banking Spoke VNet (10.10.1.0/24)                             │
│       │   │   └── ERP Spoke VNet (10.10.2.0/24)                                        │
│       │   └── Online (Public-Facing Internet Ingress Spokes)                           │
│       │                                                                                │
│       ├── SANDBOXES (Isolated Playgrounds with Zero Corporate Connectivity)            │
│       └── DECOMMISSIONED (Quarantine before subscription deletion)                     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🌐 3. Hub-and-Spoke Network Topology vs. Azure Virtual WAN

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              HUB-AND-SPOKE ENTERPRISE TOPOLOGY                          │
│                                                                                         │
│   HUB VNET (Connectivity Subscription)                                                  │
│   ┌─────────────────────────────────────────────────────────────────────────────────┐   │
│   │ • Azure Firewall Premium (Central Egress / Ingress Inspection)                  │   │
│   │ • ExpressRoute / VPN Gateway (Hybrid Tunnel to On-Premises)                     │   │
│   │ • Azure Bastion Subnet (Secure RDP/SSH Management)                              │   │
│   │ • Private DNS Zones (privatelink.blob.core.windows.net, etc.)                   │   │
│   └───────────────▲─────────────────────────────────────────────────▲───────────────┘   │
│                   │ Bidirectional VNet Peering                      │                   │
│                   ▼                                                 ▼                   │
│   SPOKE 1: PRODUCTION WORKLOADS                     SPOKE 2: NON-PRODUCTION WORKLOADS  │
│   ┌───────────────────────────────┐                 ┌───────────────────────────────┐   │
│   │ Spoke VNet (10.10.0.0/16)     │                 │ Spoke VNet (10.20.0.0/16)     │   │
│   │ • Subnet: App (10.10.1.0/24)  │                 │ • Subnet: Dev (10.20.1.0/24)  │   │
│   │ • Subnet: DB  (10.10.2.0/24)  │                 │ • Subnet: QA  (10.20.2.0/24)  │   │
│   │ • User Defined Route (UDR):   │                 │ • User Defined Route (UDR):   │   │
│   │   0.0.0.0/0 ➔ Azure Firewall  │                 │   0.0.0.0/0 ➔ Azure Firewall  │   │
│   └───────────────────────────────┘                 └───────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

> **Security Rule:** Spoke VNets do **NOT** peer directly with one another. All cross-spoke and internet-bound traffic is routed through the central Azure Firewall inside the Hub via **User-Defined Routes (UDR)**!

---

## 🤖 4. Subscription Vending Machines

In mature enterprise operations, creating a new subscription is fully automated via CI/CD:

```text
Developer submits PR requesting "New Workload: Risk-Analytics"
   │
   ▼
Pipeline triggers Subscription Vending Machine (Bicep / Terraform)
   ├── 1. Proactively provisions new Azure Subscription under MCA agreement
   ├── 2. Moves subscription into "Landing Zones / Corporate" Management Group
   ├── 3. Deploys standard Spoke Virtual Network with pre-allocated non-overlapping CIDR
   ├── 4. Peers Spoke VNet with Hub VNet; injects UDR pointing 0.0.0.0/0 to Firewall
   ├── 5. Links Spoke to Central Private DNS Zones
   └── 6. Grants Developer team RBAC Contributor rights on the new subscription!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Azure Well Architected Framework 5 Pillars](./05-Azure-Well-Architected-Framework-5-Pillars.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
