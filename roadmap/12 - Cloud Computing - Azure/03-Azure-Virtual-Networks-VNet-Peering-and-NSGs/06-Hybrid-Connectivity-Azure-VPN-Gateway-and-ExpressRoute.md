# 06 - Hybrid Connectivity: Azure VPN Gateway vs. Azure ExpressRoute

> **Focus:** Enterprise hybrid networking connecting on-premises datacenters to Azure VNets. Covers Site-to-Site (S2S) VPN, Point-to-Site (P2S) VPN, ExpressRoute circuits, Private vs. Microsoft Peering, BGP routing, Active-Active high availability, and FastPath acceleration.

---

## 🧭 1. What, Why, and History of Azure Hybrid Connectivity

### What is Hybrid Connectivity?
Hybrid connectivity establishes private, encrypted, or dedicated high-bandwidth network pipelines bridging an enterprise's physical on-premises datacenter or branch office directly into an Azure Virtual Network (VNet), allowing VMs and services to communicate over private RFC 1918 IP addresses as if they were in the same physical building.

### Why do Enterprises Need Hybrid Networking?
1. **Phased Cloud Migration:** Large enterprises take years to migrate legacy monolithic systems; web tiers reside in Azure while database backends temporarily remain on-premises.
2. **Data Sovereignty & Regulatory Compliance:** Highly sensitive patient records or banking data must legally reside on physical local servers while analytics run in Azure.
3. **Disaster Recovery & Hybrid Backup:** Replicating on-premises VMware/Hyper-V machines or SQL Server databases into Azure Storage with low latency.

### 📜 History & Evolution:
- **2012:** Microsoft introduces Site-to-Site (S2S) VPN Gateway over public internet using IPsec/IKE tunnels.
- **2014:** Azure ExpressRoute is launched at TechEd, allowing enterprises to bypass the public internet entirely using private dedicated fiber circuits via telco exchange partners.
- **2018:** Active-Active dual-redundant VPN Gateways and BGP dynamic route propagation become standard.
- **2019:** ExpressRoute Direct (100 Gbps dual ports) and FastPath (bypassing the gateway CPU to route directly to VM NICs) are released for hyper-scale workloads.

---

## ⚖️ 2. Architectural Comparison: VPN Gateway vs. ExpressRoute

| Feature / Criteria | Azure VPN Gateway | Azure ExpressRoute |
| :--- | :--- | :--- |
| **Transmission Medium** | Encrypted IPsec tunnel over the **Public Internet** | **Private Dedicated Leased Circuit** bypassing public internet |
| **Maximum Bandwidth** | Up to **10 Gbps** (VpnGw5AZ SKU) | Up to **100 Gbps** (ExpressRoute Direct) |
| **Network Latency** | Variable; subject to public internet routing & ISP congestion | Deterministic, ultra-low, predictable sub-millisecond |
| **Security / Encryption** | Encrypted in-flight with **IPsec / IKEv2 (AES-256)** | Private physical isolation (MACsec optional for Layer 2 encryption) |
| **Availability SLA** | **99.95%** (Single) / **99.99%** (Zone-redundant AZ SKU) | **99.95%** (Dual MSEE enterprise edge routers standard) |
| **Deployment Time** | ~30 to 45 minutes | Several days to weeks (requires Telco provider provisioning) |
| **Cost Profile** | Low (Hourly gateway fee + standard outbound data transfer) | Higher (Fixed monthly port fee + Telco provider circuit fee) |
| **Primary Use Cases** | Small/medium offices, dev/test, initial migrations, backup path | Mission-critical enterprise ERP, SAP, petabyte data replication |

---

## 🏗️ 3. Azure VPN Gateway Architecture

Azure VPN Gateway is deployed inside a dedicated, isolated subnet named strictly **`GatewaySubnet`** (minimum `/27` prefix).

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                               AZURE VPN GATEWAY ARCHITECTURE                           │
│                                                                                        │
│   ON-PREMISES DATACENTER                               AZURE VIRTUAL NETWORK (VNET)    │
│   ┌─────────────────────┐                              ┌───────────────────────────┐   │
│   │ On-Prem VPN Router  │                              │ Subnet: GatewaySubnet     │   │
│   │ (Cisco / Fortinet)  │                              │ ┌───────────────────────┐ │   │
│   │ Public IP: 203.0.113.1                             │ │ Azure VPN Gateway VM1 │ │   │
│   │ Local Network GW    │                              │ │ (Instance 0)          │ │   │
│   └──────────┬──────────┘                              │ └───────────┬───────────┘ │   │
│              │                                         │             │             │   │
│              │      IPsec/IKE Tunnel (Encrypted)       │ ┌───────────▼───────────┐ │   │
│              ├═════════════════════════════════════════╡ │ Azure VPN Gateway VM2 │ │   │
│              │      Public Internet Routing            │ │ (Instance 1 - Standby)│ │   │
│              │                                         │ └───────────────────────┘ │   │
│              ▼                                         └─────────────┬─────────────┘   │
│   ┌─────────────────────┐                                            │                 │
│   │ On-Prem Servers     │                                            ▼                 │
│   │ (10.0.0.0/16)       │                                      Workload Subnets        │
│   └─────────────────────┘                                      (172.16.0.0/16)         │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 🛡️ High Availability Models:
1. **Active-Passive (Default):** Two gateway VMs deployed; primary handles traffic while secondary remains standby. Failover takes ~60–90 seconds.
2. **Active-Active (Production Recommended):** Both gateway instances have distinct public IPs and maintain simultaneous active IPsec tunnels. Traffic load-balances dynamically via BGP; failover is instantaneous ($<1\text{ s}$).
3. **Zone-Redundant Gateways (`VpnGw1AZ` to `VpnGw5AZ`):** Deployed across distinct physical Availability Zones, surviving entire datacenter outages.

---

## ⚡ 4. Azure ExpressRoute Circuit Architecture

ExpressRoute provides private Layer 2 or Layer 3 connectivity through a cloud exchange co-location facility (e.g., Equinix, Megaport, AT&T, Verizon):

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE EXPRESSROUTE ARCHITECTURE                           │
│                                                                                        │
│   CUSTOMER PREMISES          TELCO PARTNER FACILITY          MICROSOFT ENTERPRISE EDGE │
│   ┌───────────────────┐      ┌───────────────────────┐       ┌───────────────────────┐ │
│   │ Customer Routers  │=====>│ Partner Cloud Router  │======>│ MSEE Primary Router   │ │
│   │ (Primary/Secondary│      │ (Exchange Provider)   │       │ MSEE Secondary Router │ │
│   └───────────────────┘      └───────────────────────┘       └───────────┬───────────┘ │
│                                                                          │             │
│   ┌──────────────────────────────────────────────────────────────────────┴─────────┐   │
│   │ EXPRESSROUTE PEERING TYPES:                                                    │   │
│   │ 1. Private Peering: Direct connection to Azure VNets (RFC 1918 private IPs).    │   │
│   │ 2. Microsoft Peering: Direct connection to public PaaS services & M365         │   │
│   │    (Storage, Azure SQL, Power BI) with BGP route filtering & NAT.              │   │
│   └────────────────────────────────────────────────────────────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 🚀 Key ExpressRoute Technologies:
- **ExpressRoute FastPath:** Bypasses the GatewaySubnet CPU entirely; routes incoming data packets straight from the MSEE edge routers into the destination VM's network card, cutting latency in half and supporting 100 Gbps.
- **ExpressRoute Global Reach:** Connects two geographically distant on-premises datacenters across Microsoft's global fiber backbone via two ExpressRoute circuits.

---

## 💻 5. Hands-on CLI: Deploying a VPN Gateway & Virtual Network

```bash
# 1. Create a Resource Group and Virtual Network
az group create --name rg-hybrid-prod --location eastus
az network vnet create --name vnet-hub --resource-group rg-hybrid-prod --address-prefix 10.100.0.0/16 --subnet-name snet-workloads --subnet-prefix 10.100.1.0/24

# 2. Create the strictly required GatewaySubnet
az network vnet subnet create --name GatewaySubnet --resource-group rg-hybrid-prod --vnet-name vnet-hub --address-prefix 10.100.255.0/27

# 3. Request a standard Public IP for the VPN Gateway
az network public-ip create --name pip-vpngw --resource-group rg-hybrid-prod --allocation-method Static --sku Standard --zone 1 2 3

# 4. Deploy the Zone-Redundant Active VPN Gateway (VpnGw1AZ)
az network vnet-gateway create \
  --name vpngw-hub-prod \
  --resource-group rg-hybrid-prod \
  --vnet vnet-hub \
  --public-ip-address pip-vpngw \
  --gateway-type Vpn \
  --vpn-type RouteBased \
  --sku VpnGw1AZ \
  --no-wait
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Azure Bastion and Private Endpoints](./05-Azure-Bastion-and-Private-Endpoints.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
