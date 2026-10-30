# 05 - Volume Expansion and Reclaim Policies

## 1. Online Volume Expansion

When `allowVolumeExpansion: true` is configured on the StorageClass, you can expand an active volume without recreating the PVC or restarting the pod:

```bash
# 1. Edit PVC to increase capacity
kubectl patch pvc db-data -p '{"spec":{"resources":{"requests":{"storage":"100Gi"}}}}'

# 2. Check PVC condition:
# Status transitions: Resizing ──► FileSystemResizePending ──► Ready
```

---

## 2. Reclaim Policies (`Retain` vs `Delete`)

- **`Delete` (Default):** When the PVC is deleted, the backing physical cloud volume (e.g. AWS EBS disk) is **permanently destroyed**.
- **`Retain` (Production Best Practice):** When the PVC is deleted, the PV transitions to `Released`. The underlying cloud disk remains untouched, allowing SREs to salvage data manually.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Access Modes ReadWriteOnce ReadWriteMany and Block Volumes](./04-Access-Modes-ReadWriteOnce-ReadWriteMany-and-Block-Volumes.md) | [Index](../../../README.md) | [06 - VolumeSnapshots and Stateful Backup Workflows →](./06-VolumeSnapshots-and-Stateful-Backup-Workflows.md) |
