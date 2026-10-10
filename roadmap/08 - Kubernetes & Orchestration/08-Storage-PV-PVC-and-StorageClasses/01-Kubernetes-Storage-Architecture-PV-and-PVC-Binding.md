# 01 - Kubernetes Storage Architecture: PV and PVC Binding

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to the Kubernetes persistent storage subsystem, contrasting cluster-scoped PersistentVolumes (PV) with namespace-scoped PersistentVolumeClaims (PVC), binding algorithms, access modes, and reclaim policies.

---

## 🏛️ 1. What Is Persistent Storage in Kubernetes?

By default, container filesystems are ephemeral. If a container writes files to its local filesystem and crashes, the container's writable layer is destroyed and all state is permanently lost.

While ephemeral storage is suitable for stateless web servers, enterprise production applications require durable, stateful persistence:
- Relational databases (PostgreSQL, MySQL) must retain database tables across pod restarts and node failures.
- Search clusters (Elasticsearch) must preserve multi-gigabyte document indices.
- Content Management Systems (WordPress, Drupal) require shared file stores for uploaded assets.

To provide durable storage while preserving the separation of concerns between infrastructure operators and application developers, Kubernetes designed a two-tiered abstraction model: **PersistentVolumes (PV)** and **PersistentVolumeClaims (PVC)**.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   PV AND PVC ABSTRACTION ARCHITECTURE                  │
└────────────────────────────────────────────────────────────────────────┘

 [ Cluster Infrastructure Administrator ]
   Provisions physical storage resource (AWS EBS, Azure Disk, NFS, Ceph)
   ├── Creates: PersistentVolume (PV)
   └── Scope: CLUSTER-SCOPED (Like a Node; exists outside namespaces)
                             │
                             │ (1-to-1 Exclusive Binding Loop)
                             ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │                     PERSISTENTVOLUME CONTROLLER                      │
 │  Evaluates Capacity, AccessModes, and StorageClass to bind PV to PVC!│
 └───────────────────────────┬──────────────────────────────────────────┘
                             │
                             ▲
 [ Application Developer ]   │
   Declares storage requirements in Pod manifest
   ├── Creates: PersistentVolumeClaim (PVC)
   └── Scope: NAMESPACE-SCOPED (Belongs to a specific namespace)
```

---

## ⚙️ 2. PersistentVolumes (PV) vs. PersistentVolumeClaims (PVC)

Understanding the distinction between PVs and PVCs is fundamental to mastering Kubernetes storage:

### 2.1 The PersistentVolume (PV) — The Physical Asset
- **What it is:** A piece of networked or physical storage in the cluster provisioned by an administrator (static provisioning) or dynamically by a StorageClass plugin.
- **Cluster-Scoped Resource:** A PV is **not** bound to any namespace. It exists at the cluster level, identical to a worker `Node`.
- **Infrastructure Abstraction:** Encapsulates the specific underlying storage technology (e.g., AWS EBS Volume ID, NFS server IP and export path, Ceph pool).

### 2.2 The PersistentVolumeClaim (PVC) — The Consumer's Voucher
- **What it is:** A declarative request for storage submitted by an application developer.
- **Namespace-Scoped Resource:** A PVC lives inside a specific namespace alongside the Pod that consumes it.
- **Developer Abstraction:** The developer does not need to know whether the storage is backed by an AWS EBS drive, an Azure Managed Disk, or an on-premise NetApp SAN. The developer simply requests: *"I need 50 GiB of storage with ReadWriteOnce access."*

---

## 🔄 3. The 1-to-1 Exclusive Binding Mechanism

The **PersistentVolume Controller** in the control plane continuously watches for newly created PVCs and searches for matching PVs:

1. **Strict 1-to-1 Relationship:** A PersistentVolumeClaim binds to **exactly one** PersistentVolume. Once a PV is bound to a PVC, it cannot be bound to any other PVC simultaneously, even if the PVC only requested 10 GiB of a 100 GiB volume!
2. **Matching Criteria:** The controller binds a PV to a PVC only if:
   - The PV's capacity is greater than or equal to the PVC's requested capacity ($\text{PV Capacity} \ge \text{PVC Request}$).
   - The PV supports the requested **Access Modes**.
   - Both resources share the exact same `storageClassName`.
   - The PV matches any label selectors defined on the PVC.

---

## 🚦 4. The Four PV Lifecycle Phases

Every PersistentVolume transitions through four discrete operational phases:

| Lifecycle Phase | Operational State & Working Mechanism |
| :--- | :--- |
| **`Available`** | The volume is provisioned, healthy, and ready to be bound, but no claim currently owns it. |
| **`Bound`** | The volume has been successfully and exclusively bound to a specific PersistentVolumeClaim. |
| **`Released`** | The PVC was deleted by the user, but the physical cluster storage has not yet been reclaimed. The volume contains old data and cannot be bound to a new claim until cleared. |
| **`Failed`** | The automatic volume reclamation process encountered an error. |

---

## 🛡️ 5. Reclaim Policies (`persistentVolumeReclaimPolicy`)

When a developer finishes with a workload and deletes a PersistentVolumeClaim (`kubectl delete pvc`), what happens to the underlying physical data? The answer is governed by the volume's **Reclaim Policy**:

1. **`Retain` (Default for Manually Provisioned PVs):**
   - Deleting the PVC leaves the PersistentVolume and its underlying physical data **completely intact**.
   - The PV transitions to the `Released` status.
   - **Why this matters in Enterprise:** Prevents accidental catastrophic data loss! An administrator must manually inspect the volume, back up the files, and delete the resource.
2. **`Delete` (Default for Dynamic StorageClasses):**
   - Deleting the PVC triggers the automatic destruction of both the Kubernetes PV resource **and the underlying cloud storage asset** (e.g., permanently deletes the AWS EBS volume or GCP Persistent Disk).
3. **`Recycle` (Deprecated):**
   - Performed a basic `rm -rf` wipe on the underlying storage directory to make it available again. Deprecated in favor of dynamic provisioning.

---

## 📂 6. Access Modes Explained

A volume can be mounted with specific access capabilities depending on the capabilities of the underlying storage hardware:

| Access Mode | CLI Shorthand | Working Mechanism & Supported Media |
| :--- | :---: | :--- |
| **ReadWriteOnce** | `RWO` | Can be mounted as read-write by **a single worker node at a time**. Supported by standard cloud block storage (AWS EBS, Azure Disk, GCP Persistent Disk). Multiple pods can mount it *only* if they are scheduled on the exact same worker node! |
| **ReadOnlyMany** | `ROX` | Can be mounted as read-only by **multiple worker nodes simultaneously**. Useful for shared read-only reference data or static website assets. |
| **ReadWriteMany** | `RWX` | Can be mounted as read-write by **multiple worker nodes simultaneously**. Requires network-attached distributed file systems (NFS, AWS EFS, Azure Files, CephFS, GlusterFS). |
| **ReadWriteOncePod** | `RWOP` | (Kubernetes v1.27+ GA). Can be mounted as read-write by **a single Pod across the entire cluster**. Strictly enforces single-pod write exclusivity even on multi-pod nodes. |

---

## 📝 7. Complete Manifest: Manual PV, PVC, and Pod Mounting

```yaml
# 1. Cluster-Scoped PersistentVolume (Defined by Admin)
apiVersion: v1
kind: PersistentVolume
metadata:
  name: enterprise-nfs-pv
spec:
  capacity:
    storage: 100Gi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual-storage
  nfs:
    server: 192.168.1.200
    path: /exports/production-data

---
# 2. Namespace-Scoped PersistentVolumeClaim (Defined by Developer)
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-storage-pvc
  namespace: production
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: manual-storage
  resources:
    requests:
      storage: 50Gi

---
# 3. Pod Mounting the PVC
apiVersion: v1
kind: Pod
metadata:
  name: stateful-database-pod
  namespace: production
spec:
  containers:
  - name: postgres
    image: postgres:16-alpine
    volumeMounts:
    - name: data-volume
      mountPath: /var/lib/postgresql/data
  volumes:
  - name: data-volume
    persistentVolumeClaim:
      claimName: database-storage-pvc
```

---

## 📋 8. Summary and Navigation

The Kubernetes persistent storage architecture cleanly separates infrastructure allocation from workload consumption. By decoupling cluster-level PersistentVolumes from application PersistentVolumeClaims, enforcing deterministic 1-to-1 binding loops, and offering granular access modes, Kubernetes enables stateful enterprise workloads to run with high availability and data durability.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (07-ConfigMaps-and-Secrets)](../07-ConfigMaps-and-Secrets/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - StorageClasses and Dynamic Provisioning →](./02-StorageClasses-and-Dynamic-Provisioning.md) |
