# 01 - Azure Kubernetes Service (AKS) Architecture: Managed Control Plane & Node Pools

> **Focus:** Enterprise managed Kubernetes, Free Microsoft-managed control plane, System Node Pools vs. User Node Pools, and VMSS worker node backing.

---

## ☸️ 1. AKS Architecture

**Azure Kubernetes Service (AKS)** simplifies deploying a managed Kubernetes cluster in Azure by offloading the operational overhead to Microsoft:

```text
┌────────────────────────────────────────────────────────┐
│             MICROSOFT-MANAGED CONTROL PLANE            │
│  (kube-apiserver, etcd, kube-scheduler, kube-cont.)    │
│  • Fully managed, patched, and monitored by Microsoft  │
│  • FREE standard control plane (or paid 99.95% SLA)    │
└───────────────────────────┬────────────────────────────┘
                            │ Azure CNI / VNet Subnet
                            ▼
┌────────────────────────────────────────────────────────┐
│                 CUSTOMER DATA PLANE (VNet)             │
│                                                        │
│  ┌─────────────────────────┐  ┌─────────────────────┐  │
│  │    System Node Pool     │  │   User Node Pool    │  │
│  │ (Critical cluster pods: │  │ (Application apps,  │  │
│  │  CoreDNS, metrics-server│  │  microservices,     │  │
│  │  runs on Linux VMSS)    │  │  Linux or Windows)  │  │
│  └─────────────────────────┘  └─────────────────────┘  │
└────────────────────────────────────────────────────────┘
```

---

## 🧱 2. System Node Pools vs. User Node Pools

- **System Node Pool:** Dedicated to core Kubernetes cluster components (`kube-system` namespace). Must be Linux. Microsoft protects system pods using taints and tolerations.
- **User Node Pool:** Dedicated to your business workloads. Can run Linux (Ubuntu or Azure Linux) or Windows Server containers, GPU VM sizes, or Spot instances.

---

### 💻 Production Azure CLI AKS Cluster Creation

```bash
# 1. Create AKS Cluster with System Pool
az aks create \
  --resource-group rg-containers \
  --name aks-production-cluster \
  --node-count 3 \
  --node-vm-size Standard_D4s_v5 \
  --enable-managed-identity \
  --generate-ssh-keys

# 2. Add an Autoscaling User Node Pool
az aks nodepool add \
  --resource-group rg-containers \
  --cluster-name aks-production-cluster \
  --name userapps \
  --node-count 2 \
  --min-count 2 \
  --max-count 10 \
  --enable-cluster-autoscaler \
  --node-vm-size Standard_E4s_v5

# 3. Get Credentials for kubectl
az aks get-credentials --resource-group rg-containers --name aks-production-cluster
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (06-Azure-SQL-CosmosDB-and-Managed-Databases)](../06-Azure-SQL-CosmosDB-and-Managed-Databases/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - AKS Networking Kubenet vs Azure CNI →](./02-AKS-Networking-Kubenet-vs-Azure-CNI.md) |
