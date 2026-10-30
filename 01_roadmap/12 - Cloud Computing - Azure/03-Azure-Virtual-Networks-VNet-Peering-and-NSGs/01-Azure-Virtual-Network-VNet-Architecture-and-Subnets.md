# 01 - Azure Virtual Network (VNet) Architecture, Subnets, & Peering

A Virtual Network (VNet) is the fundamental building block of private networking in Microsoft Azure. It is a logically isolated software-defined network in the Azure cloud where virtual machines, container clusters, databases, and application gateways communicate securely with each other, with the internet, and with on-premises data centers.

---

## 🏗️ 1. VNet Architecture & Subnetting Topology

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        AZURE VIRTUAL NETWORK (VNet) TOPOLOGY                           │
│                                                                                        │
│   AZURE REGION (e.g., East US)                                                         │
│   VNet Address Space: 10.0.0.0/16 (65,536 Total IP Addresses)                         │
│                                                                                        │
│   ┌───────────────────────────────────┐     ┌───────────────────────────────────┐      │
│   │ SUBNET-WEB (10.0.1.0/24)          │     │ SUBNET-DATA (10.0.2.0/24)         │      │
│   │ • 251 Usable Host IPs             │     │ • 251 Usable Host IPs             │      │
│   │ • Web API VMs / App Gateways      │     │ • PostgreSQL / Redis Databases    │      │
│   │ • Attached: NSG-Web (Port 80/443) │     │ • Attached: NSG-DB (Port 5432)    │      │
│   └─────────────────┬─────────────────┘     └─────────────────▲─────────────────┘      │
│                     │                                         │                        │
│                     └───────────────────┬─────────────────────┘                        │
│                                         │ Internal Azure Backbone Routing              │
│   ┌─────────────────────────────────────┴───────────────────────────────────────────┐  │
│   │ SPECIALIZED SUB-NETWORKS                                                        │  │
│   │ • GatewaySubnet (10.0.255.0/27)  ──► Required for VPN Gateway & ExpressRoute    │  │
│   │ • AzureBastionSubnet (10.0.254.0/26)► Secure browser-based SSH/RDP jump box      │  │
│   └─────────────────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔒 2. The 5 Reserved IP Addresses per Subnet

In traditional IPv4 networking, only 2 addresses per subnet are reserved (the network address and the broadcast address). **In Azure, exactly 5 IP addresses are reserved in every subnet**:

| Reserved IP | Role / Purpose | Technical Behavior in Azure |
| :--- | :--- | :--- |
| **`x.x.x.0`** | **Network Address** | Standard IPv4 network identifier. |
| **`x.x.x.1`** | **Default Gateway** | Automatically assigned by Azure as the default router for all outgoing subnet packets. |
| **`x.x.x.2`** | **Azure DNS (Primary)** | Resolves Azure internal DNS queries (maps to Azure virtual IP `168.63.129.16`). |
| **`x.x.x.3`** | **Azure DNS (Secondary)**| Secondary redundant DNS resolver. |
| **`x.x.x.255`**| **Broadcast Address** | Standard IPv4 broadcast address. (Azure networks are non-broadcast, but address is reserved). |

### Capacity Calculation Formula
For any subnet with CIDR prefix length $N$:
$$\text{Usable Host IPs} = 2^{(32 - N)} - 5$$

- For a `/24` subnet: $256 - 5 = \mathbf{251\text{ usable IPs}}$ (starting from `.4` through `.254`).
- For a `/28` subnet: $16 - 5 = \mathbf{11\text{ usable IPs}}$.

---

## 🛑 3. Specialized Dedicated Subnets

Certain Azure services require a dedicated, strictly named subnet and prohibit deploying standard VMs or other resources into them:

| Subnet Name | Minimum CIDR | Mandatory Purpose & Constraints |
| :--- | :---: | :--- |
| **`GatewaySubnet`** | `/27` (preferred `/26`) | Hosts Azure VPN Gateway and ExpressRoute virtual network gateway instances. |
| **`AzureBastionSubnet`**| `/26` | Hosts the managed Azure Bastion service for secure browser-based SSH/RDP access. |
| **`AzureFirewallSubnet`**| `/26` | Dedicated to the central stateful Azure Firewall appliance. |
| **`AppGatewaySubnet`** | `/27` | Dedicated subnet for Azure Application Gateway (Layer 7 WAF). |

---

## 🌐 4. VNet Peering: Hub-and-Spoke Architecture

VNet Peering seamlessly connects two Azure Virtual Networks using Microsoft's high-speed global fiber backbone. Once peered, VMs in different VNets communicate with single-digit millisecond latency using private IP addresses.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        HUB-AND-SPOKE VNET PEERING TOPOLOGY                             │
│                                                                                        │
│   SPOKE VNET 1 (Web Tier)                                 SPOKE VNET 2 (DB Tier)       │
│   CIDR: 10.1.0.0/16                                       CIDR: 10.2.0.0/16            │
│   [VM-Web1: 10.1.1.4]                                     [VM-DB1: 10.2.1.4]           │
│           ▲                                                       ▲                    │
│           │                                                       │                    │
│           ▼ (Peering 1: Private Backbone)                         ▼ (Peering 2)        │
│   ┌──────────────────────────────────────────────────────────────────────────────┐     │
│   │ HUB VNET (Central Connectivity & Security): CIDR 10.0.0.0/16                 │     │
│   │ • Azure Firewall (Inspection)   • GatewaySubnet (VPN to Corporate On-Prem)   │     │
│   └──────────────────────────────────────────────────────────────────────────────┘     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Critical Peering Architectural Rules
1. **Peering is NOT Automatically Bi-Directional:** You must create the peering in **both directions** (Hub ➔ Spoke AND Spoke ➔ Hub) for communication to function.
2. **Peering is Non-Transitive:** If Spoke 1 is peered to Hub, and Spoke 2 is peered to Hub, **Spoke 1 CANNOT talk to Spoke 2** unless traffic is explicitly routed through a Network Virtual Appliance (NVA) / Azure Firewall in the Hub or peered directly (Spoke 1 ↔ Spoke 2).
3. **No Overlapping CIDR Blocks:** VNets with overlapping IP ranges (e.g., both using `10.0.0.0/16`) cannot be peered.

---

## 🛡️ 5. Connecting to PaaS: Service Endpoints vs. Private Endpoints

| Architectural Dimension | Service Endpoints | Private Endpoints (Azure Private Link) |
| :--- | :--- | :--- |
| **Traffic Destination** | Public IP of the Azure PaaS service via private Azure backbone. | **Private IP address inside your own VNet subnet** (e.g., `10.0.1.25`). |
| **Public Internet Exposure** | PaaS resource maintains a public DNS endpoint (locked by firewall). | **Zero Public Access:** Public IP can be completely disabled. |
| **DNS Resolution** | Resolves to public IP of service. | Resolves to private IP via Azure Private DNS Zone. |
| **Cross-VNet / On-Premises** | Cannot be reached from on-premises over VPN or ExpressRoute. | Fully accessible from on-premises over VPN and ExpressRoute. |
| **Cost** | **100% Free** (No additional charge). | Billed per hour for Private Endpoint + data processing fee per GB. |

---

## 💻 6. Production Azure CLI Provisioning Recipes

```bash
# 1. Create a Resource Group
az group create --name rg-networking --location eastus

# 2. Create the VNet with /16 Address Space
az network vnet create \
  --resource-group rg-networking \
  --name vnet-prod \
  --address-prefix 10.0.0.0/16 \
  --location eastus

# 3. Create Web Subnet
az network vnet subnet create \
  --resource-group rg-networking \
  --vnet-name vnet-prod \
  --name subnet-web \
  --address-prefix 10.0.1.0/24

# 4. Create Database Subnet with Service Endpoint for Storage
az network vnet subnet create \
  --resource-group rg-networking \
  --vnet-name vnet-prod \
  --name subnet-db \
  --address-prefix 10.0.2.0/24 \
  --service-endpoints Microsoft.Storage

# 5. Create GatewaySubnet for VPN connectivity
az network vnet subnet create \
  --resource-group rg-networking \
  --vnet-name vnet-prod \
  --name GatewaySubnet \
  --address-prefix 10.0.255.0/27

# 6. Establish VNet Peering in Both Directions
# Leg A: vnet-hub -> vnet-spoke
az network vnet peering create \
  --resource-group rg-networking \
  --name hub-to-spoke \
  --vnet-name vnet-hub \
  --remote-vnet vnet-spoke \
  --allow-vnet-access

# Leg B: vnet-spoke -> vnet-hub
az network vnet peering create \
  --resource-group rg-networking \
  --name spoke-to-hub \
  --vnet-name vnet-spoke \
  --remote-vnet vnet-hub \
  --allow-vnet-access
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (02-Azure-Entra-ID-and-Role-Based-Access-Control-RBAC)](../02-Azure-Entra-ID-and-Role-Based-Access-Control-RBAC/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Network Security Groups NSG and Application Security Groups ASG →](./02-Network-Security-Groups-NSG-and-Application-Security-Groups-ASG.md) |
