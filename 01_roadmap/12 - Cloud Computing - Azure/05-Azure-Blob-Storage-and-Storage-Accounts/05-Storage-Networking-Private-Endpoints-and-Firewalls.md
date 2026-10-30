# 05 - Storage Networking: Private Endpoints, Service Endpoints & Storage Firewalls

> **Focus:** Securing Azure Storage network boundaries. Deep dive into public endpoint disabling, Storage Account Firewalls, Service Endpoints vs. Azure Private Link, Private DNS Zone integration (`privatelink.blob.core.windows.net`), and data exfiltration defense.

---

## 🌐 1. What, Why, and History of Azure Storage Networking

### What is Storage Networking?
Storage Networking encompasses the security perimeters, route controls, and private network interfaces used to restrict access to Azure Storage accounts so that data requests cannot originate from the open public internet.

### Why Must Default Public Access Be Eliminated?
By default, every Azure Storage account exposes a globally resolvable Public FQDN (e.g., `mystorage.blob.core.windows.net`) bound to a public Microsoft IP address.
1. **Data Exfiltration Threat:** Malicious insiders or compromised workloads can exfiltrate corporate databases to unauthorized personal storage accounts if outbound internet access is open.
2. **Denial of Service / Brute Force:** Publicly accessible endpoints are constantly probed by internet port scanners and automated credential credential-stuffing bots.
3. **Regulatory Mandates:** PCI-DSS 4.0, HIPAA, and ISO 27001 require backend data stores to be isolated within non-routable private subnets.

### 📜 History & Evolution:
- **2010:** Azure Storage accounts only offered open public IP endpoints protected solely by Access Keys.
- **2017:** **Virtual Network Service Endpoints** introduced, allowing subnets with service tag `Microsoft.Storage` to communicate over Microsoft's internal backbone while keeping public DNS endpoints.
- **2019:** **Azure Private Link & Private Endpoints** launched. Storage accounts now receive dedicated private IP addresses (NICs) inside customer subnets, completely disabling public routing.
- **2021:** Capability to set **Public Network Access = Disabled** (`publicNetworkAccess = "Disabled"`), dropping all non-Private-Endpoint traffic at Microsoft's edge.

---

## ⚖️ 2. Architectural Comparison: Service Endpoints vs. Private Endpoints

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              STORAGE NETWORKING COMPARISON                             │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ OPTION A: SERVICE ENDPOINTS (Routing Optimization)                                     │
│   VM (10.0.1.4) ──────> Microsoft Backbone ──────> Storage Account (Public IP)         │
│   • Source IP is rewritten to Azure service tag at the gateway.                        │
│   • Traffic remains on Microsoft backbone, but target remains a public IP.             │
│   • Does NOT support on-premises VPN/ExpressRoute access.                              │
│                                                                                        │
│ OPTION B: PRIVATE ENDPOINTS / PRIVATE LINK (True Network Isolation)                    │
│   VM (10.0.1.4) ──────> Subnet NIC (10.0.2.5) ───> Storage Container (Private Link)   │
│   • Storage gets an internal virtual NIC with a private RFC 1918 IP address.           │
│   • Public internet routing is completely severed.                                     │
│   • Accessible from On-Premises datacenters via VPN / ExpressRoute!                    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Feature | Virtual Network Service Endpoints | Azure Private Endpoints (Private Link) |
| :--- | :--- | :--- |
| **IP Address of Storage** | Public Azure IP address | **Private RFC 1918 IP** inside customer subnet |
| **On-Premises Reachability**| ❌ No (Cannot route via VPN/ExpressRoute)| ✅ **Yes** (Fully routable across hybrid links) |
| **Data Exfiltration Defense**| Moderate (Can apply Service Endpoint Policies)| **Maximum** (Only specific resource is mapped to IP)|
| **DNS Configuration** | Standard public DNS (`blob.core.windows.net`) | Requires **Private DNS Zone** (`privatelink.blob.core.windows.net`)|
| **Cost** | Free (Included with VNet) | Inexpensive hourly endpoint fee + data processing fee |

---

## 🗺️ 3. Private Endpoint DNS Resolution Architecture

When a Private Endpoint is provisioned, Azure creates a CNAME alias redirecting traffic from the public hostname to the private link zone:

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                 DNS RESOLUTION FLOW                                     │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. Application queries: mystorage.blob.core.windows.net                                 │
│ 2. Azure Public DNS returns CNAME: mystorage.privatelink.blob.core.windows.net         │
│ 3. VNet Resolver queries Azure Private DNS Zone (privatelink.blob.core.windows.net)     │
│ 4. Private DNS Zone resolves to internal Private IP: 10.0.2.5                           │
│ 5. Application establishes TLS connection directly to 10.0.2.5 on port 443!             │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical Implementation: Locking Down Storage with Azure CLI

```bash
# 1. Create a Storage Account with Public Access disabled
az storage account create \
  --name storagesecureprod \
  --resource-group rg-storage \
  --location eastus \
  --sku Standard_ZRS \
  --public-network-access Disabled

# 2. Create the Private Endpoint inside the application subnet
az network private-endpoint create \
  --name pe-storage-blob \
  --resource-group rg-storage \
  --vnet-name vnet-corp \
  --subnet snet-data \
  --private-connection-resource-id $(az storage account show -n storagesecureprod -g rg-storage --query id -o tsv) \
  --group-id blob \
  --connection-name conn-pe-blob

# 3. Create Private DNS Zone and link to VNet
az network private-dns zone create \
  --resource-group rg-storage \
  --name "privatelink.blob.core.windows.net"

az network private-dns link vnet create \
  --resource-group rg-storage \
  --zone-name "privatelink.blob.core.windows.net" \
  --name "vnet-corp-link" \
  --virtual-network vnet-corp \
  --registration-enabled false

# 4. Register the DNS record for the Private Endpoint
az network private-endpoint dns-zone-group create \
  --resource-group rg-storage \
  --endpoint-name pe-storage-blob \
  --name "blob-dns-zone-group" \
  --private-dns-zone "privatelink.blob.core.windows.net" \
  --zone-name "blob"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Storage Security Shared Access Signatures SAS and Keys](./04-Storage-Security-Shared-Access-Signatures-SAS-and-Keys.md) | [Index](../../../README.md) | [06 - Azure Files Azure Disks and Storage Sync →](./06-Azure-Files-Azure-Disks-and-Storage-Sync.md) |
