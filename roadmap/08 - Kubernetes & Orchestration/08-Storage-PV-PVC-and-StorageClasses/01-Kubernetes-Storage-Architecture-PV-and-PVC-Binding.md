# 01 - Kubernetes Storage Architecture: PV and PVC Binding

## 1. Storage Abstraction Layer

Kubernetes decouples storage infrastructure from application developers through a two-layer abstraction:
1. **PersistentVolume (PV):** A piece of physical storage in the cluster provisioned by an administrator or dynamically by a StorageClass (Cluster-scoped).
2. **PersistentVolumeClaim (PVC):** A request for storage by a developer specifying size and access mode (Namespace-scoped).

```text
[ Developer creates PVC ] ──► Matches capacity & access mode ──► [ PersistentVolume (PV) ]
          │                                                               │
          ▼                                                               ▼
   Attached to Pod:                                              Mounted to Cloud Disk
spec.volumes[*].persistentVolumeClaim                        (AWS EBS, Azure Disk, GPD, NFS)
```

---

## 2. PV Lifecycle Phases

- **`Available`:** Free resource that is not yet bound to a claim.
- **`Bound`:** The volume is bound to a specific PVC.
- **`Released`:** The claim was deleted, but the physical cluster storage has not yet been reclaimed.
- **`Failed`:** Automatic reclamation failed.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (07-ConfigMaps-and-Secrets)](../07-ConfigMaps-and-Secrets/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - StorageClasses and Dynamic Provisioning →](./02-StorageClasses-and-Dynamic-Provisioning.md) |
