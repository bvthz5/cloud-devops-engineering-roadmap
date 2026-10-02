# 🔷 Azure Services & Architecture Types — Kids-Mind Short Notes

> **Format:** Simple 1-line definitions, real-life analogies, and visual comparison tables. Covers every core Azure service and all internal architecture types (VM series, Managed Disk types, Blob Storage access tiers, Load Balancing & Networking types, Entra ID RBAC, and Cosmos DB models).

---

## 🧭 1. Azure Virtual Machine (VM) Series & Sizing Explained Simply

Azure VM sizes use a clear naming convention:

```text
       ┌─── Tier: "Standard" (Production grade)
       │         ┌─── Family / Series: "D" = General Purpose
       │         │ ┌─ vCPU count: "4" = 4 vCPUs
       │         │ │ ┌─ Feature: "s" = Premium SSD disk capable
       │         │ │ │ ┌─ Generation: "v5" = Version 5 (Latest Intel/AMD)
       ▼         ▼ ▼ ▼ ▼
    Standard_    D 4 s _ v 5
```

### 🏷️ Sub-letter Feature Decoder:
| Letter | Feature | Meaning |
| :---: | :--- | :--- |
| **`s`** | Premium SSD Capable | Supports high-speed Premium SSD and Ultra Disks (e.g., `D4s_v5`). |
| **`a`** | AMD Processor | Built on AMD EPYC CPUs; slightly lower cost. |
| **`p`** | ARM64 Ampere Altra | ARM-based compute; extreme energy and cost efficiency. |
| **`m`** | High Memory | Extra RAM allocation compared to base series. |
| **`d`** | Local Scratch Disk | Includes a fast temporary local temp disk attached to the physical host. |

---

### 💻 Master Azure VM Series Table:

| VM Series | Family Name | Kids-Mind 1-Line Definition | Real-Life Analogy | Best Use Cases |
| :---: | :--- | :--- | :--- | :--- |
| **B-Series** | **Burstable Economy** | Very cheap VMs that accumulate credits when idle to burst up to 100% CPU on demand. | A scooter with a turbo boost button for occasional steep hills. | Dev/test servers, low-traffic web blogs, build agents, small databases. |
| **D-Series** | **General Purpose** | Balanced enterprise VMs with a reliable 1:4 vCPU to RAM ratio. | A trusted daily commuter SUV; reliable for almost everything. | Production web APIs, medium relational databases, microservice workloads. |
| **E-Series** | **Memory Optimized** | Massive RAM allocations (1:8 vCPU to RAM ratio) for giant in-memory computing. | A high-capacity cruise ship designed to hold massive passenger loads. | SAP HANA, Redis caches, high-transaction relational databases (SQL Server, Oracle). |
| **F-Series** | **Compute Optimized** | High CPU clock speeds with a 1:2 vCPU to RAM ratio for raw calculation throughput. | A high-speed electric sports car built for pure acceleration. | Batch image processing, web analytics engines, financial modeling, gaming servers. |
| **N-Series** | **GPU Accelerated** | High-performance workstations equipped with NVIDIA GPUs (NC, ND, NV sub-series). | An IMAX film rendering and visual effects studio computer. | AI / LLM deep learning training, video rendering, AutoCAD engineering workstations. |
| **L-Series** | **Storage Optimized** | VMs bundled with high-throughput, low-latency direct local NVMe SSD disks. | A commercial freight transport vehicle built for direct heavy loading. | High-performance NoSQL (Cassandra, MongoDB), data warehousing, Elasticsearch. |

---

## 💽 2. Azure Managed Disk Types

Azure Managed Disks are virtual hard drives attached to Azure VMs:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                      AZURE MANAGED DISK TIERS                          │
│                                                                        │
│   [Standard HDD] ──► [Standard SSD] ──► [Premium SSD v1/v2] ──► [Ultra]│
│     60 MB/s            750 MB/s           1,200 MB/s          10,000   │
│     Dev/Backups        Web Servers        Production DBs      Extreme  │
└────────────────────────────────────────────────────────────────────────┘
```

| Disk Type | Media | Max IOPS | Max Throughput | Latency | Kids-Mind Summary & Best Use |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Standard HDD** | Magnetic | 2,000 | 500 MB/s | ~10–20 ms | Cheap magnetic disk. Best for non-critical backups, dev/test VMs, and logs. |
| **Standard SSD** | Entry SSD | 6,000 | 750 MB/s | Single-digit ms | Consistent entry-level SSD. Ideal for dev environments, light web servers, test workloads. |
| **Premium SSD (v1)** | High-perf SSD | 20,000 | 900 MB/s | Sub-millisecond | Standard enterprise production disk. Best for production databases and critical systems. |
| **Premium SSD v2** | Next-Gen SSD | **80,000** | **1,200 MB/s** | Sub-millisecond | **Flexible provisioning!** Customize disk size, IOPS, and MB/s independently without resizing disk! |
| **Ultra Disk** | Extreme SSD | **400,000** | **10,000 MB/s** | **Sub-millisecond** | Extreme top-tier performance for I/O heavy workloads (SAP HANA, high-frequency finance). |
| **Ephemeral OS Disk** | Host Cache/RAM | Hardware speed | Hardware speed | Near Zero | OS disk created directly on the physical host machine; re-imaged on VM reboot. **Zero storage cost!** Best for stateless AKS worker nodes. |

---

## 🪣 3. Azure Blob Storage Access Tiers & Redundancy

Azure Blob Storage stores unformatted files, videos, documents, and backups:

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                   AZURE BLOB STORAGE ACCESS TIERS                        │
│                                                                          │
│   [Hot Tier] ──────► [Cool Tier] ──────► [Cold Tier] ──────► [Archive]   │
│   Frequent reads     Monthly reads       Quarterly reads     Deep backup │
│   Highest storage $  Lower storage $     Even lower $        Lowest $    │
│   Lowest read $      Higher read fee     Higher read fee     Highest read│
└──────────────────────────────────────────────────────────────────────────┘
```

| Access Tier | Minimum Stored Duration | Retrieval Latency | 1-Line Definition (Kids Mind) |
| :--- | :---: | :---: | :--- |
| **Hot Tier** | None | Immediate (ms) | Active daily files, live website media, active user uploads. |
| **Cool Tier** | 30 days | Immediate (ms) | Cheaper storage for data accessed at least once a month (e.g., monthly billing statements). |
| **Cold Tier** | 90 days | Immediate (ms) | Much cheaper storage for data accessed once every 3 months; moderate retrieval fee. |
| **Archive Tier** | 180 days | **Hours (Rehydration)** | **Offline deep storage;** cheapest price per GB. Takes up to 15 hours to "rehydrate" back to Hot. |

### 🛡️ Azure Storage Redundancy Options:
- **LRS (Locally Redundant Storage):** 3 copies within a single data center. Protects against disk crashes, but not against the building flooding.
- **ZRS (Zone-Redundant Storage):** 3 copies spread across 3 distinct Availability Zones in the same region. Protects against a full data center failure.
- **GRS (Geo-Redundant Storage):** 3 copies in primary region + 3 copies copied asynchronously to a secondary region hundreds of miles away!
- **GZRS (Geo-Zone-Redundant Storage):** Maximum safety! 3 AZs in primary region + secondary region geo-replication.

---

## ⚖️ 4. Azure Load Balancing & Traffic Routing Services

Azure has 4 distinct traffic distribution services:

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                     AZURE LOAD BALANCER DECISION MATRIX                  │
│                                                                          │
│                 Is your traffic Global (Multi-Region)?                   │
│                     ├── YES ──► Is it HTTP(S)?                           │
│                     │             ├── YES ──► [Azure Front Door] (L7)    │
│                     │             └── NO  ──► [Azure Traffic Mgr] (DNS)  │
│                     └── NO (Regional) ──► Is it HTTP(S)?                 │
│                                   ├── YES ──► [Application Gateway] (L7) │
│                                   └── NO  ──► [Azure Load Balancer] (L4) │
└──────────────────────────────────────────────────────────────────────────┘
```

| Service | OSI Layer | Scope | Key Features | 1-Line Definition (Kids Mind) |
| :--- | :---: | :---: | :--- | :--- |
| **Azure Load Balancer** | **Layer 4** (TCP/UDP) | Regional | Ultra-low latency, millions of requests, private & public IP options, Basic vs Standard SKU. | High-speed network switch forwarding raw TCP/UDP traffic to backend VMs. |
| **Application Gateway** | **Layer 7** (HTTP/HTTPS)| Regional | URL path routing (`/images`, `/api`), SSL termination, integrated **Web Application Firewall (WAF)**. | Smart web traffic conductor directing browser requests to specific backend microservices. |
| **Azure Front Door** | **Layer 7** (HTTP/HTTPS)| **Global** | Global Anycast edge network, built-in global CDN, SSL offloading, global WAF, instant failover. | Worldwide acceleration highway that connects users to the closest global Microsoft edge PoP. |
| **Traffic Manager** | **DNS-based** | **Global** | DNS resolution directing user to nearest regional IP based on latency, priority, or geographic weights. | Intelligent global phonebook pointing user DNS queries to the healthiest regional server. |

---

## 🌐 5. Azure Networking & Hybrid Connectivity

| Networking Component | 1-Line Definition (Kids Mind) | Real-Life Analogy |
| :--- | :--- | :--- |
| **Azure VNet (Virtual Network)** | An isolated private network in your Azure subscription. | Your private fenced residential plot in the cloud. |
| **Subnet** | A segmented block of IP addresses inside a VNet. | Dividing your land into rooms (living room, bedroom). |
| **VNet Peering** | Direct Microsoft backbone connection between two VNets (no internet transit). | A private hallway connecting two neighboring apartments. |
| **NSG (Network Security Group)** | A 5-tuple stateful firewall filter attached to subnets or VM NICs. | The security guard at your building door checking IDs. |
| **ASG (Application Security Group)** | Grouping VMs by functional label (e.g., `WebServers`) to write clean NSG rules without hardcoding IP addresses! | Giving all hotel staff the same badge so one door rule lets them all in. |
| **Azure Bastion** | Fully managed PaaS jump host providing secure browser-based RDP/SSH into VMs without exposing public IPs. | A secure visitor lobby with glass viewing booths. |
| **Azure VPN Gateway** | Encrypted tunnel across the public internet between on-prem and Azure (Site-to-Site or Point-to-Site). | An armored security car driving through public streets. |
| **Azure ExpressRoute** | Private dedicated physical circuit bypassing the public internet directly into Microsoft's cloud. | A private underground high-speed subway line from your office directly to Azure. |

---

## 🏛️ 6. Azure Governance Hierarchy & Identity (Entra ID / RBAC)

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                       AZURE GOVERNANCE HIERARCHY                         │
│                                                                          │
│   [Root Management Group]                                                │
│             │                                                            │
│             ▼                                                            │
│   [Management Groups] (Department: Finance, Engineering, HR)             │
│             │                                                            │
│             ▼                                                            │
│   [Subscriptions] (Dev-Sub, Prod-Sub, Billing Boundary)                  │
│             │                                                            │
│             ▼                                                            │
│   [Resource Groups] (Logical folder: rg-ecommerce-prod)                  │
│             │                                                            │
│             ▼                                                            │
│   [Resources] (VMs, Disks, VNets, Storage Accounts, Key Vaults)          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 🔐 Microsoft Entra ID (Formerly Azure AD) & Azure RBAC:
- **Entra ID:** Identity provider managing human users, groups, application registrations, and authentication (SSO, MFA, Conditional Access).
- **Azure RBAC (Role-Based Access Control):** Permissions assigned to identities at a specific hierarchy scope.
- **Top 4 Core Built-In Roles:**
  - **Owner:** Full access to all resources, including the ability to grant access to others.
  - **Contributor:** Full access to create and manage resources, but **cannot** grant permissions to others.
  - **Reader:** View-only access; cannot change or create anything.
  - **User Access Administrator:** Manages user permissions without managing the actual infrastructure.

### 🤖 Azure Managed Identities (Zero Hardcoded Credentials!):
- **System-Assigned:** Tied directly to one single Azure resource (e.g., a VM). Automatically deleted if the VM is deleted.
- **User-Assigned:** Created as an independent standalone identity asset and assigned to multiple VMs or functions.

---

## 🗄️ 7. Azure Database & Container Types

| Service | Architecture Type | Key Superpower | When to Choose |
| :--- | :---: | :--- | :--- |
| **Azure SQL Database** | PaaS Relational | 99.995% SLA, automated backups, AI query tuning. | Modern cloud-native apps needing serverless or provisioned SQL Server. |
| **Azure SQL Managed Instance** | Full Instance PaaS | Near 100% compatibility with on-premises SQL Server (SQL Agent, linked servers). | "Lift-and-shift" enterprise SQL Server migration without rewriting code. |
| **Azure Cosmos DB** | Multi-Model Globally Distributed NoSQL | **Sub-10ms response times anywhere on Earth;** 99.999% SLA; multi-region active-active writes. | Global e-commerce carts, IoT device telemetry, gaming profiles. Supports **SQL, MongoDB, Cassandra, Gremlin (Graph), Table APIs**. |
| **Azure Database for PostgreSQL / MySQL** | Fully Managed Open Source | Automated patching, high availability across AZs, read replicas. | Open-source web stacks (Django, Node.js, Spring Boot, Laravel). |
| **AKS (Azure Kubernetes Service)** | Managed K8s | Free control plane; integrates with Azure CNI, Entra ID, and Azure Monitor. | Enterprise microservices requiring full Kubernetes API compatibility. |
| **Azure Container Apps (ACA)** | Serverless Containers | Built on Kubernetes + KEDA + Envoy; scales to zero; pay per second. | Microservices without the complexity of managing a full Kubernetes cluster. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← AWS Services & Types Short Notes](./09-AWS-Services-and-Types-Deep-Dive-Short-Notes.md) | [Index](../README.md) | [Web Servers & Reverse Proxies Short Notes →](./11-Web-Servers-and-Reverse-Proxies-Short-Notes.md) |
