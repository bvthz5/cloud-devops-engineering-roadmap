# 03 - Azure VNet Peering: Regional vs. Global Peering & Non-Transitive Routing

> **Focus:** High-speed private network interconnectivity, Regional VNet Peering vs. Global VNet Peering, Gateway Transit, and Non-Transitive routing constraints.

---

## 🔗 1. Azure VNet Peering Architecture

**VNet Peering** directly connects two Azure Virtual Networks across the Microsoft private backbone network:

```text
┌─────────────────────────┐  VNet Peering Link ┌─────────────────────────┐
│     vnet-hub-prod       │◄───────────────────►│      vnet-spoke-app     │
│   CIDR: 10.0.0.0/16     │  (Sub-ms Latency)   │    CIDR: 10.1.0.0/16    │
│   Location: East US     │                     │    Location: East US    │
└─────────────────────────┘                     └─────────────────────────┘
```

### 📊 Regional vs. Global Peering:
- **Regional VNet Peering:** Connects VNets within the **same Azure region**. Delivers ultra-low sub-millisecond latency.
- **Global VNet Peering:** Connects VNets across **different geographic regions** (e.g., East US to West Europe). Traffic never traverses the public internet; it stays entirely on Microsoft's private subsea and terrestrial fiber network.

---

## ⚠️ 2. The 3 Core Rules of VNet Peering

1. **Non-Transitive Routing:** If VNet A is peered with VNet B, and VNet B is peered with VNet C, **VNet A CANNOT communicate with VNet C** through VNet B unless an explicit Network Virtual Appliance (NVA) router with User-Defined Routes (UDRs) or Azure Virtual WAN is configured.
2. **Non-Overlapping IP Spaces:** You cannot peer two VNets that have overlapping CIDR blocks (e.g., both cannot use `10.0.0.0/16`).
3. **Bi-Directional Configuration Required:** Peering is not automatic. You must explicitly create **two peering links** (A $ightarrow$ B and B $ightarrow$ A) for traffic to flow!

```bash
# 1. Peer Hub to Spoke
az network vnet peering create \
  --name hub-to-spoke \
  --resource-group rg-networking \
  --vnet-name vnet-hub-prod \
  --remote-vnet vnet-spoke-app \
  --allow-vnet-access

# 2. Peer Spoke to Hub (Required second link)
az network vnet peering create \
  --name spoke-to-hub \
  --resource-group rg-networking \
  --vnet-name vnet-spoke-app \
  --remote-vnet vnet-hub-prod \
  --allow-vnet-access
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Network Security Groups NSG and Application Security Groups ASG](./02-Network-Security-Groups-NSG-and-Application-Security-Groups-ASG.md) | [Index](../../../README.md) | [04 - Azure Route Tables and User Defined Routes UDR →](./04-Azure-Route-Tables-and-User-Defined-Routes-UDR.md) |
