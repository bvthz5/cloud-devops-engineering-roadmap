# 06 - VolumeSnapshots and Stateful Backup Workflows

## 1. The VolumeSnapshot CRDs

Kubernetes provides standard CRDs to take storage-level point-in-time snapshots:
1. `VolumeSnapshotClass`: Defines the snapshot driver (e.g. `ebs.csi.aws.com`).
2. `VolumeSnapshot`: User request to snapshot a specific PVC.
3. `VolumeSnapshotContent`: The actual cluster-level snapshot stored in the storage backend.

```yaml
apiVersion: snapshot.storage.k8s.io/v1
kind: VolumeSnapshot
metadata:
  name: postgres-backup-snap
spec:
  volumeSnapshotClassName: csi-aws-snapclass
  source:
    persistentVolumeClaimName: postgres-data-pvc
```

---

## 2. Restoring from a Snapshot

To spin up a new volume from the snapshot:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-restored-pvc
spec:
  dataSource:
    name: postgres-backup-snap
    kind: VolumeSnapshot
    apiGroup: snapshot.storage.k8s.io
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 50Gi
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Volume Expansion](./05-Volume-Expansion-and-Reclaim-Policies.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
