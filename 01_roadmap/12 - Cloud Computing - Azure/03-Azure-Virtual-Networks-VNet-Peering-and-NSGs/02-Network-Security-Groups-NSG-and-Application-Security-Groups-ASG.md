# 02 - Network Security Groups (NSGs), Application Security Groups (ASGs), Load Balancers & Traffic Management

> **Focus:** Stateful packet filtering, NSG priority evaluation (100–4096), default security rules, Application Security Groups (ASGs), Layer 4 Azure Load Balancer vs. Layer 7 Application Gateway, Azure DNS, and global traffic routing via Azure Traffic Manager.

---

## 🛡️ 1. Network Security Groups (NSG) Deep Dive

A **Network Security Group (NSG)** is Azure's stateful Layer 4 firewall that filters inbound and outbound network traffic to Azure resources inside a Virtual Network (VNet).

```text
Internet
   │
   ▼
┌────────────────────────────────────────────────────────┐
│               NSG (Subnet-Level Scope)                 │
│  Rule 100: Allow HTTPS (TCP 443) Inbound               │
│  Rule 110: Allow SSH (TCP 22) from Admin IP Inbound    │
│  Rule 120: Allow HTTP (TCP 80) Inbound                 │
│  Rule 4000: Explicit Deny All Other Inbound            │
│  Rule 65500: Default DenyAllInBound                    │
└───────────────────────────┬────────────────────────────┘
                            │
              ┌─────────────▼──────────────┐
              │     Subnet: subnet-web     │
              │  ┌──────────────────────┐  │
              │  │ VM-Web1 (NIC-Web1)   │  │
              │  │                      │  │
              │  │ ◄── NIC-Level NSG ── │  │ (Optional secondary defense layer)
              │  └──────────────────────┘  │
              └────────────────────────────┘
```

### ⚙️ NSG Rule Properties
Every rule in an NSG is evaluated using a 5-tuple mechanism:

| Property | Description | Permitted Values / Constraints |
| :--- | :--- | :--- |
| **Priority** | Order in which rules are processed. Lower number = higher priority. | `100` to `4096` (Integers). |
| **Direction** | Specifies whether the rule inspects incoming or outgoing packets. | `Inbound` or `Outbound`. |
| **Access** | The security action taken when packet criteria match. | `Allow` or `Deny`. |
| **Protocol** | The transport layer protocol to match. | `Tcp`, `Udp`, `Icmp`, or `*` (Any). |
| **Source / Destination** | The origin or target address filter. | Single IP (CIDR), IP range, Service Tag (e.g., `Internet`, `VirtualNetwork`), or ASG. |
| **Source / Destination Port** | Port or range of ports to inspect. | Single port (e.g., `443`), range (e.g., `8080-8085`), or `*` (Any). |

> 💡 **Stateful Inspection Rule:** NSG rules are **stateful**. If an inbound packet is permitted by an NSG rule, the return response outbound packet is automatically allowed regardless of outbound security rules (and vice-versa).

---

### 📋 Default Security Rules (Built-in & Unremovable)
Every NSG is instantiated with 6 default rules. They have the lowest priority (`65000`–`65500`) so custom rules (`100`–`4096`) always override them:

| Priority | Rule Name | Direction | Access | Source ➔ Destination | Technical Purpose |
| :---: | :--- | :---: | :---: | :--- | :--- |
| **65000** | `AllowVnetInBound` | Inbound | Allow | `VirtualNetwork` ➔ `VirtualNetwork` | Allows all internal communication between subnets and peered VNets. |
| **65001** | `AllowAzureLoadBalancerInBound` | Inbound | Allow | `AzureLoadBalancer` ➔ `*` | Allows Azure health probe monitors to query backend servers. |
| **65500** | `DenyAllInBound` | Inbound | **Deny** | `*` ➔ `*` | **Zero-Trust Default**: Blocks all external incoming traffic from the internet. |
| **65000** | `AllowVnetOutBound` | Outbound | Allow | `VirtualNetwork` ➔ `VirtualNetwork` | Permits VMs to send traffic to other resources within the VNet. |
| **65001** | `AllowInternetOutBound` | Outbound | Allow | `*` ➔ `Internet` | Permits VMs to download OS updates and reach public endpoints. |
| **65500** | `DenyAllOutBound` | Outbound | **Deny** | `*` ➔ `*` | Final safety barrier blocking non-VNet/non-Internet destinations. |

---

### 💻 Production Azure CLI NSG Commands

```bash
# 1. Create an NSG
az network nsg create   --resource-group rg-networking   --name nsg-web   --location eastus

# 2. Allow HTTPS (Port 443) from anywhere
az network nsg rule create   --resource-group rg-networking   --nsg-name nsg-web   --name Allow-HTTPS   --priority 100   --direction Inbound   --access Allow   --protocol Tcp   --source-address-prefixes "*"   --source-port-ranges "*"   --destination-address-prefixes "*"   --destination-port-ranges 443

# 3. Allow SSH (Port 22) restricted to corporate bastion / admin IP only
az network nsg rule create   --resource-group rg-networking   --nsg-name nsg-web   --name Allow-SSH-AdminOnly   --priority 110   --direction Inbound   --access Allow   --protocol Tcp   --source-address-prefixes "203.0.113.10/32"   --source-port-ranges "*"   --destination-address-prefixes "*"   --destination-port-ranges 22

# 4. Associate the NSG to the Subnet
az network vnet subnet update   --resource-group rg-networking   --vnet-name vnet-prod   --name subnet-web   --network-security-group nsg-web

# 5. Inspect Effective Security Rules on a VM NIC (Critical for debugging!)
az network nic list-effective-nsg   --resource-group rg-networking   --name nic-vm1   --output table
```

---

## 🏷️ 2. Application Security Groups (ASGs)

**Application Security Groups (ASGs)** eliminate the nightmare of hardcoding IP addresses into firewall rules. They group VM network interfaces logically by application tier.

```text
Without ASG (Hardcoded IPs - Fragile):
  Allow Inbound Port 1433: Source = 10.0.1.4, 10.0.1.5, 10.0.1.6...

With ASG (Declarative & Resilient):
  Allow Inbound Port 1433: Source = asg-web-servers  ➔  Destination = asg-database-servers
```

```bash
# Create ASGs for Web Tier and Database Tier
az network asg create --resource-group rg-networking --name asg-web --location eastus
az network asg create --resource-group rg-networking --name asg-db --location eastus

# Attach VM NIC to the Web ASG
az network nic ip-config update   --resource-group rg-networking   --nic-name nic-vm-web1   --name ipconfig1   --application-security-groups asg-web

# Write an NSG rule referencing ASGs
az network nsg rule create   --resource-group rg-networking   --nsg-name nsg-db   --name Allow-Web-to-DB   --priority 100   --direction Inbound   --access Allow   --protocol Tcp   --source-asgs asg-web   --source-port-ranges "*"   --destination-asgs asg-db   --destination-port-ranges 1433
```

---

## ⚖️ 3. Azure Load Balancer (Layer 4) vs. Application Gateway (Layer 7)

```text
┌──────────────────────────────────────────────┐       ┌──────────────────────────────────────────────┐
│        Azure Load Balancer (Layer 4)         │       │          Application Gateway (Layer 7)       │
│                                              │       │                                              │
│  • Operates at TCP/UDP Transport Layer       │       │  • Operates at HTTP/HTTPS Application Layer  │
│  • Ultra-high throughput, sub-ms latency     │       │  • URL path routing (/api/*, /images/*)      │
│  • Direct packet distribution                │       │  • SSL/TLS Termination offload               │
│  • No URL inspection or cookie affinity      │       │  • Web Application Firewall (WAF) OWASP      │
└──────────────────────────────────────────────┘       └──────────────────────────────────────────────┘
```

| Feature | Azure Load Balancer | Azure Application Gateway |
| :--- | :--- | :--- |
| **OSI Layer** | **Layer 4** (TCP / UDP) | **Layer 7** (HTTP / HTTPS / WebSocket / HTTP/2) |
| **Routing Decisions** | IP address + Port (5-tuple hash) | Host header, URL path, HTTP cookies, query string |
| **SSL/TLS Termination**| ❌ No (Pass-through only) | ✅ Yes (Centralized certificate offloading) |
| **Security / WAF** | ❌ No WAF capabilities | ✅ Yes (WAF_v2 SKU with OWASP CRS 3.2 prevention) |
| **Cookie Affinity** | ❌ No | ✅ Yes (Sticky sessions via gateway-managed cookie) |
| **Subnet Requirement**| Any standard subnet | **Dedicated Subnet Required** (No other resources) |
| **Public & Internal** | Both supported | Both supported |
| **SKUs Available** | Basic (Deprecated) vs. **Standard** | Standard_v2 vs. **WAF_v2** |

> ⚠️ **Exam & Production Rule:** **Standard Load Balancer** enforces a **Secure By Default** model: backend VMs MUST have an NSG explicitly allowing incoming health probe and traffic requests.

---

## 🌐 4. Azure DNS & Azure Traffic Manager

```text
User in London (Europe)                   User in Tokyo (Asia)
          │                                         │
          ▼                                         ▼
┌────────────────────────────────────────────────────────┐
│             Azure Traffic Manager (DNS-Based)          │
│              contoso.trafficmanager.net                │
│                                                        │
│  Routing Method: Performance (Latency-Optimized)       │
└───────────────────────────┬────────────────────────────┘
              ┌─────────────┴──────────────┐
              │                            │
              ▼ DNS: London App IP         ▼ DNS: Tokyo App IP
     ┌─────────────────┐          ┌─────────────────┐
     │ App Service     │          │ App Service     │
     │ (West Europe)   │          │ (East Asia)     │
     └─────────────────┘          └─────────────────┘
```

### 🗺️ Traffic Manager Routing Methods
Azure Traffic Manager is a **DNS-based global traffic router**. It does **not** proxy or inspect traffic packets; it merely answers DNS queries with the optimal IP address:

1. **Performance**: Routes to the endpoint with the lowest network latency relative to the user's ISP.
2. **Priority**: Directs 100% traffic to primary endpoint; automatically fails over to secondary on probe failure.
3. **Weighted**: Distributes traffic proportionally across endpoints based on configured weights (canary testing).
4. **Geographic**: Directs users based on the geographic origin of their DNS query (Data sovereignty).
5. **Multivalue**: Returns multiple healthy endpoints in a single DNS response for client-side resiliency.
6. **Subnet**: Maps specific user source IP address blocks directly to designated endpoints.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure Virtual Network VNet Architecture and Subnets](./01-Azure-Virtual-Network-VNet-Architecture-and-Subnets.md) | [Index](../../../README.md) | [03 - VNet Peering Global vs Local Peering →](./03-VNet-Peering-Global-vs-Local-Peering.md) |
