# 01 - Azure Virtual Machine Series, Sizing, Disks & Lifecycle Management

> **Focus:** IaaS compute fundamentals, VM size naming convention, VM family classifications (B, D, E, F, L, N), OS image marketplace, Managed Disks breakdown (OS, Temp, Data), and compute lifecycle (Stop vs. Deallocate).

---

## 💻 1. Azure Virtual Machines (VM) Architecture

An Azure Virtual Machine is an **Infrastructure as a Service (IaaS)** resource. Azure provides the physical datacenter hardware, hypervisor, and network connectivity; you own and manage the Operating System, middleware, runtime environment, application code, and data.

```text
┌────────────────────────────────────────────────────────┐
│                   Azure Virtual Machine                │
│                                                        │
│  ┌────────────────┐ ┌────────────────┐ ┌─────────────┐ │
│  │    OS Disk     │ │   Data Disk    │ │  Temp Disk  │ │
│  │ (Managed SSD)  │ │ (Managed SSD)  │ │(Local Host) │ │
│  │ Persistent     │ │ Persistent     │ │ Ephemeral   │ │
│  └────────────────┘ └────────────────┘ └─────────────┘ │
│          │                  │                 │        │
└──────────┼──────────────────┼─────────────────┼────────┘
           ▼                  ▼                 ▼
  Azure Managed Disks Storage Subsystem     Host RAM/SSD
```

---

## 🏷️ 2. Azure VM Size Naming Syntax Decoded

Azure uses a structured naming syntax for all VM SKUs:

```text
         Standard_D4s_v3
         │        │││  │
         │        │││  └── Generation: Version 3 (Processor microarchitecture)
         │        ││└───── Sub-feature: "s" = Premium SSD capable
         │        │└────── vCPU Count: 4 Virtual CPUs
         │        └─────── Family: "D" = General Purpose
         └──────────────── Pricing Tier: "Standard" (Production Grade)
```

### 🔠 Sub-letter Attribute Table:
- **`s`**: Premium SSD / Ultra Disk capable (e.g., `Standard_D4s_v5`). Non-`s` sizes only support Standard HDD/SSD.
- **`a`**: AMD EPYC processor based (cost-optimized).
- **`p`**: ARM64 processor based (Ampere Altra CPUs with exceptional power and cost efficiency).
- **`m`**: Memory-intensive SKU variant (higher RAM per vCPU).
- **`d`**: Includes a fast local ephemeral scratch disk on the physical hypervisor host.

---

## 📊 3. Master Azure VM Families Comparison Matrix

| Family | Series | vCPU : RAM Ratio | Real-World Workload Suitability | Analogy |
| :--- | :--- | :---: | :--- | :--- |
| **General Purpose** | **B, D, Dsv** | `1 : 4` | Web servers, dev/test environments, small to medium databases, CI/CD runners. | Daily commuter SUV; versatile and dependable. |
| **Compute Optimized** | **F, Fsv** | `1 : 2` | High CPU-to-memory workloads, batch processing, web analytics engines, video transcoding. | Lightweight sports car built for pure acceleration. |
| **Memory Optimized** | **E, Edsv, M** | `1 : 8` | Large in-memory relational databases (SQL Server, Oracle), SAP HANA, Redis caching clusters. | High-capacity cargo ship built to transport massive loads. |
| **Storage Optimized** | **L, Lsv** | `1 : 8` + NVMe | Low-latency local NVMe I/O, NoSQL databases (Cassandra, MongoDB), Elasticsearch clusters. | Flatbed transport freight truck designed for direct heavy cargo. |
| **GPU Accelerated** | **NC, ND, NV** | Dedicated GPU | Deep learning / LLM AI training, 3D CAD visualization, seismic simulation, video rendering. | Specialized movie studio rendering farm. |
| **Burstable Economy** | **B-Series** | Dynamic | Small workloads that stay idle and burst up to 100% CPU when traffic arrives using accumulated CPU credits. | Scooter with an on-demand turbo booster button. |

---

## 💽 4. VM Disks Architecture: OS vs. Data vs. Temp

Every Azure VM includes or supports three distinct disk types:

| Disk Type | Persistent? | Default Location | Description & Best Practice |
| :--- | :---: | :--- | :--- |
| **OS Disk** | **Yes** | Azure Managed Disks | Boots the operating system. Max size 4 TB (typically 30–128 GB). Registered as `/dev/sda` (Linux) or `C:` (Windows). |
| **Temporary Disk** | ❌ **NO** | Physical Hypervisor Host | Fast local NVMe/SSD on the host machine. **DATA IS LOST ON STOP/DEALLOCATE**. Used solely for pagefiles, swap, and tempdb. |
| **Data Disk** | **Yes** | Azure Managed Disks | Dedicated virtual disks attached for databases and application files. Max 32–64 TB each. Up to 64 disks per VM. |

---

## 🛑 5. Critical Compute Lifecycle: Stopped vs. Deallocated

Understanding compute state transitions is mandatory for cost optimization:

```text
   [Running] ──(OS Shutdown or az vm stop)──► [Stopped / Allocated]
                                                    │  (Still occupies host CPU/RAM)
                                                    │  (COMPUTE CHARGES CONTINUE!)
                                                    ▼
                                           [Deallocated]
                                           (Compute released)
                                           (COMPUTE CHARGES DROP TO $0!)
                                           (Only pay for persistent disk storage)
```

- **Stopped (Allocated):** Triggered when shutting down the VM from inside the OS (`sudo shutdown -h now`). The hypervisor host retains the CPU/RAM hardware allocation. **You are still billed for full compute hourly rates.**
- **Deallocated:** Triggered via Azure Portal or CLI (`az vm deallocate`). Azure releases the hardware back to the datacenter pool. **Compute billing halts completely.** You only pay small storage fees for the persistent Managed Disks.

---

### 💻 Production Azure CLI VM Deployment

```bash
# 1. Create a Resource Group
az group create --name demo-rg --location eastus

# 2. Deploy an Ubuntu 22.04 LTS VM (B2s Burstable)
az vm create   --resource-group demo-rg   --name my-ubuntu-vm   --image Ubuntu2204   --size Standard_B2s   --admin-username azureuser   --generate-ssh-keys

# 3. Open Port 22 for SSH Management
az vm open-port --port 22 --resource-group demo-rg --name my-ubuntu-vm

# 4. Deallocate VM when not in use (Stops compute billing!)
az vm deallocate --resource-group demo-rg --name my-ubuntu-vm

# 5. Start VM again
az vm start --resource-group demo-rg --name my-ubuntu-vm
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (03-Azure-Virtual-Networks-VNet-Peering-and-NSGs)](../03-Azure-Virtual-Networks-VNet-Peering-and-NSGs/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Azure Managed Disks Ultra Premium Standard SSD →](./02-Azure-Managed-Disks-Ultra-Premium-Standard-SSD.md) |
