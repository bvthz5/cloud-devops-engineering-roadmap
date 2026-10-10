# 05 - AKS Storage Architecture: CSI Drivers, Azure Disks, Azure Files & PVCs

> **Focus:** Stateful storage in Azure Kubernetes Service (AKS). Deep dive into Container Storage Interface (CSI) drivers (`disk.csi.azure.com`, `file.csi.azure.com`, `blob.csi.azure.com`), Access Modes (`ReadWriteOnce` vs. `ReadWriteMany`), StorageClasses, and dynamic PersistentVolumeClaim provisioning.

---

## 💽 1. What, Why, and History of AKS Storage

### What is AKS Persistent Storage?
Kubernetes pods are ephemeral by design; when a pod crashes or is rescheduled to another worker node, all data written to its local container layer is permanently erased. AKS Persistent Storage connects external, durable Azure storage infrastructure directly to pods inside the cluster.

### Why do We Need CSI Storage Drivers?
1. **Stateful Workloads:** Mission-critical databases (PostgreSQL, MongoDB, Kafka, Redis) require persistent state that survives pod restarts and node upgrades.
2. **Shared Multi-Pod Access:** CMS applications (WordPress, Drupal) and log aggregators require multiple pods across different nodes to simultaneously read and write to the same shared filesystem.
3. **Automated Lifecycle Management:** Developers declare storage requirements in Kubernetes manifests (`PersistentVolumeClaims`), and the storage driver automatically calls Azure Resource Manager APIs to provision, attach, and format the underlying Azure Managed Disk or File Share without cluster admin intervention.

### 📜 History & Evolution:
- **Pre-v1.21 (In-Tree Volume Plugins):** Storage code was compiled directly into the Kubernetes core binary (`kubernetes.io/azure-disk`). Upgrading or fixing a bug in Azure storage required upgrading the entire Kubernetes cluster version.
- **v1.21+ (CSI Driver Standard):** Microsoft transitioned AKS to out-of-tree **Container Storage Interface (CSI) drivers**. Storage logic now runs as independent DaemonSets and controller pods.
- **Current Standard (v1.26+):** In-tree drivers completely removed; CSI drivers (`disk.csi.azure.com`, `file.csi.azure.com`, `blob.csi.azure.com`) provide dynamic expansion, volume snapshots, and Azure Key Vault CSI secret driver integration.

---

## ⚖️ 2. Architectural Comparison: Azure Disks vs. Azure Files vs. Azure Blob

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AKS STORAGE DRIVER DECISION TREE                          │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Does your workload need high IOPS and block storage (Databases)?                       │
│ └──> YES: Azure Disk CSI (disk.csi.azure.com) ➔ ReadWriteOnce (RWO)                    │
│                                                                                        │
│ Do multiple pods across different worker nodes need concurrent read/write access?      │
│ └──> YES: Azure Files CSI (file.csi.azure.com) ➔ ReadWriteMany (RWX via SMB / NFS)     │
│                                                                                        │
│ Do you need petabyte-scale low-cost media/data ingestion?                              │
│ └──> YES: Azure Blob CSI (blob.csi.azure.com) ➔ ReadWriteMany (BlobFuse / NFS v3)      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Feature / Criteria | Azure Managed Disk CSI | Azure Files CSI | Azure Blob CSI |
| :--- | :--- | :--- | :--- |
| **Driver Identifier** | `disk.csi.azure.com` | `file.csi.azure.com` | `blob.csi.azure.com` |
| **Supported Access Modes**| **`ReadWriteOnce` (RWO)** only | **`ReadWriteMany` (RWX)** & RWO | **`ReadWriteMany` (RWX)** |
| **Protocol** | Block Storage (SCSI/NVMe) | File Share (SMB 3.0 / NFS 4.1) | Object Storage (REST / BlobFuse) |
| **Performance Profile** | Extreme IOPS & low sub-ms latency | Moderate IOPS, burstable | High throughput, higher latency |
| **Cross-Zone Detach/Attach**| Bound to node's Availability Zone | Zone-redundant (ZRS supported) | Zone-redundant / Geo-redundant |
| **Primary Workloads** | PostgreSQL, MySQL, Redis, Kafka | CMS uploads, shared configs, logs | Machine Learning datasets, images |

---

## 🏗️ 3. Dynamic Provisioning Workflow: StorageClass to PersistentVolumeClaim

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                DYNAMIC STORAGE LIFECYCLE                               │
│                                                                                        │
│   1. Developer submits PersistentVolumeClaim (PVC)                                     │
│      spec:                                                                             │
│        storageClassName: managed-csi-premium                                           │
│        resources: requests: storage: 100Gi                                             │
│                               │                                                        │
│                               ▼                                                        │
│   2. AKS CSI Controller catches PVC and calls Azure ARM REST API                       │
│                               │                                                        │
│                               ▼                                                        │
│   3. Azure Resource Manager provisions a 100 GiB Premium SSD Managed Disk              │
│      in the resource group 'MC_rg_aks_eastus'.                                         │
│                               │                                                        │
│                               ▼                                                        │
│   4. AKS creates PersistentVolume (PV) object and binds it to the PVC (Bound status)   │
│                               │                                                        │
│                               ▼                                                        │
│   5. Kubelet calls CSI Node Driver to attach and format the disk on the worker node    │
│      and mounts it inside the container filesystem under /var/lib/data.                │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Kubernetes Manifest Example: Stateful Database with Azure Disk

```yaml
# 1. Declare the PersistentVolumeClaim
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
  namespace: database
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: managed-csi-premium
  resources:
    requests:
      storage: 50Gi
---
# 2. Mount into PostgreSQL Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres-db
  namespace: database
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:15-alpine
          ports:
            - containerPort: 5432
          env:
            - name: POSTGRES_PASSWORD
              value: "SuperSecretAzureDB2026!"
          volumeMounts:
            - name: postgres-storage
              mountPath: /var/lib/postgresql/data
      volumes:
        - name: postgres-storage
          persistentVolumeClaim:
            claimName: postgres-pvc
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Azure Container Registry ACR and AKS Integration](./04-Azure-Container-Registry-ACR-and-AKS-Integration.md) | [Index](../../../README.md) | [06 - AKS Observability Container Insights and Prometheus Grafana →](./06-AKS-Observability-Container-Insights-and-Prometheus-Grafana.md) |
