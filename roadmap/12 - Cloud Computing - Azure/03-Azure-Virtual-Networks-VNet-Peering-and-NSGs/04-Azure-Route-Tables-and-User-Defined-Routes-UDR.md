# 04 - Azure Route Tables & User-Defined Routes (UDR)

> **Focus:** Controlling Azure network traffic flow, System Default Routes vs. User-Defined Routes (UDRs), Next Hop Types, and routing through Network Virtual Appliances (NVAs).

---

## 🗺️ 1. System Default Routes vs. User-Defined Routes

Azure automatically creates default system routes for every subnet in a VNet:
- `Local VNet Traffic`: `10.0.0.0/16` $ightarrow$ Next hop: `Virtual network`
- `Internet Outbound`: `0.0.0.0/0` $ightarrow$ Next hop: `Internet`
- `On-Premises Gateway`: Inter-premises addresses $ightarrow$ Next hop: `Virtual network gateway`

### ⚡ User-Defined Routes (UDR):
To inspect all outbound internet traffic with an enterprise firewall (e.g., Azure Firewall, Palo Alto, Fortinet), you create a custom **Route Table** and attach it to your subnets:

```text
Subnet-App (10.0.1.0/24)
         │
         ▼ (Outbound packet to 0.0.0.0/0)
┌────────────────────────────────────────────────────────┐
│             Route Table: rt-spoke-to-firewall          │
│  Prefix: 0.0.0.0/0 ➔ Next Hop Type: VirtualAppliance   │
│                      Next Hop IP: 10.0.100.4           │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│         Centralized Azure Firewall (10.0.100.4)        │
│          (Deep packet inspection & threat detection)   │
└────────────────────────────────────────────────────────┘
```

---

### 💻 Azure CLI Route Table Deployment

```bash
# 1. Create Route Table
az network route-table create \
  --name rt-egress-inspection \
  --resource-group rg-networking \
  --location eastus

# 2. Add UDR forcing all traffic to Firewall NVA
az network route-table route create \
  --resource-group rg-networking \
  --route-table-name rt-egress-inspection \
  --name ForceTrafficThroughFirewall \
  --address-prefix "0.0.0.0/0" \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address "10.0.100.4"

# 3. Associate Route Table with App Subnet
az network vnet subnet update \
  --resource-group rg-networking \
  --vnet-name vnet-prod \
  --name subnet-app \
  --route-table rt-egress-inspection
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - VNet Peering Global vs Local Peering](./03-VNet-Peering-Global-vs-Local-Peering.md) | [Index](../../../README.md) | [05 - Azure Bastion and Private Endpoints →](./05-Azure-Bastion-and-Private-Endpoints.md) |
