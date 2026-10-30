# 06 - Physical Hybrid Cloud: Azure Stack Portfolio & ExpressRoute Integration

> **Focus:** Physical hardware appliances and dedicated hybrid infrastructure in Microsoft Azure. Deep dive into the Azure Stack Portfolio (Azure Stack HCI, Azure Stack Edge, Azure Stack Hub), hardware requirements, disconnected operation, and ExpressRoute dedicated interconnects.

---

## 🏛️ 1. What, Why, and History of the Azure Stack Portfolio

### What is the Azure Stack Portfolio?
The Azure Stack Portfolio is a collection of certified hardware appliances and operating systems designed to bring native Azure compute, storage, virtualization, and AI capabilities directly into on-premises enterprise datacenters and harsh remote edge environments.

### Why do Organizations Deploy Physical Azure Stack Hardware?
1. **Air-Gapped Autonomous Operation:** Military forward bases, cruise ships, offshore oil rigs, and submarine vessels have zero internet access for months at a time, yet need modern container orchestration and VM virtualization.
2. **Local AI Inferencing at the Edge:** Manufacturing quality inspection cameras take 60 frames per second of manufactured parts. Uploading petabytes of raw video to the public cloud wastes bandwidth; Azure Stack Edge runs real-time computer vision models locally and only uploads anomalies.
3. **Data Center Modernization with HCI:** Replacing aging SAN storage arrays and VMware licensing costs with Microsoft's Hyper-Converged Infrastructure (HCI) software-defined storage.

---

## ⚖️ 2. Architectural Breakdown: Azure Stack Family Compared

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE STACK PORTFOLIO FAMILY                              │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. AZURE STACK HCI (Hyper-Converged Infrastructure):                                   │
│    • Hypervisor OS running on certified OEM servers (Dell VxRail, HPE ProLiant, Lenovo)│
│    • Combines Hyper-V virtualization, Storage Spaces Direct (S2D), and native Azure Arc│
│    • Replaces legacy SAN storage and VMware clusters with Azure subscription billing.  │
│                                                                                        │
│ 2. AZURE STACK EDGE (AI-Optimized Cloud-Managed Appliance):                            │
│    • 1U / 2U rackmount hardware shipped directly by Microsoft on monthly subscription. │
│    • Bundles NVIDIA Tesla T4 GPUs or Intel FPGAs for local real-time AI computer vision│
│    • Functions as a local network storage gateway (SMB/NFS) with automatic Azure sync. │
│                                                                                        │
│ 3. AZURE STACK HUB (Autonomous Private Cloud Appliance):                               │
│    • Self-contained, turnkey private cloud rack (4 to 16 physical nodes).              │
│    • Runs complete Azure IaaS & PaaS (App Services, SQL, Blob) 100% disconnected!      │
│    • For defense, intelligence, and sovereign national government clouds.              │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Criteria | Azure Stack HCI | Azure Stack Edge | Azure Stack Hub |
| :--- | :--- | :--- | :--- |
| **Hardware Procurement** | Customer purchases certified OEM nodes | Hardware shipped by Microsoft (Rental)| Turnkey integrated multi-rack system |
| **Primary Workloads** | Enterprise Windows/Linux VMs & AKS HCI | Edge AI inference, IoT gateway, caching | Autonomous disconnected IaaS/PaaS |
| **Cloud Connectivity** | Requires periodic sync (Within 30 days)| Connected to Azure Storage | **Supports 100% disconnected / air-gapped**|
| **Management Plane** | Azure Portal via native Azure Arc | Azure Portal & local web UI | Dedicated local Azure Stack Hub portal |
| **Typical Customer** | Enterprise IT, branch offices, datacenters| Retail stores, factory floors, medical | Defense, intelligence, maritime vessels |

---

## ⚡ 3. ExpressRoute Circuit Architecture with Azure Stack

To link on-premises Azure Stack installations with Azure public regions, enterprises provision **Azure ExpressRoute**:

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              EXPRESSROUTE HYBRID DATA HIGHWAY                           │
│                                                                                         │
│   ON-PREMISES DATACENTER                                    MICROSOFT GLOBAL CLOUD      │
│   ┌──────────────────────────────────────────────┐          ┌────────────────────────┐  │
│   │ Azure Stack HCI Cluster                      │          │ Azure Cloud Region     │  │
│   │ (Dell PowerEdge Servers - Hyper-V & S2D)     │          │ (e.g., East US)        │  │
│   └──────────────────────┬───────────────────────┘          └───────────▲────────────┘  │
│                          │ Local LAN                                    │               │
│                          ▼                                              │               │
│   ┌──────────────────────────────────────────────┐          ┌───────────┴────────────┐  │
│   │ Customer Border Router (BGP ASN: 65001)      │          │ Microsoft Enterprise   │  │
│   └──────────────────────┬───────────────────────┘          │ Edge (MSEE Routers)    │  │
│                          │                                  └───────────▲────────────┘  │
│                          │ Dual 10 Gbps / 100 Gbps                      │               │
│                          ▼ Private Fiber Cross-Connect                  │               │
│   ┌─────────────────────────────────────────────────────────────────────┴────────────┐  │
│   │ CO-LOCATION CLOUD EXCHANGE PROVIDER (Equinix / Megaport / Digital Realty)        │  │
│   │ Private Dedicated Layer 2 / Layer 3 circuit bypassing public internet completely!│  │
│   └──────────────────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Hands-on CLI: Checking ExpressRoute Circuit & Peering Status

```bash
# 1. View all ExpressRoute circuits and provisioned bandwidth
az network express-route list --output table

# 2. Check BGP peering state on an ExpressRoute circuit
az network express-route peering list \
  --circuit-name "er-corp-direct-10g" \
  --resource-group "rg-networking-core" \
  --output table

# 3. View the BGP route table advertised by the on-premises router to MSEE
az network express-route list-route-tables \
  --circuit-name "er-corp-direct-10g" \
  --resource-group "rg-networking-core" \
  --peering-name "AzurePrivatePeering" \
  --device-path "primary" \
  --output table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Azure Arc Enabled Data Services](./05-Azure-Arc-Enabled-Data-Services.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
