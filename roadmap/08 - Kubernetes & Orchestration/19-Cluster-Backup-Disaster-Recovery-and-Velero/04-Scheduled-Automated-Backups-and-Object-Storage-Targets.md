# 04 - Scheduled Automated Backups and Object Storage Targets

## 1. Production Velero Schedule Manifest

```yaml
apiVersion: velero.io/v1
kind: Schedule
metadata:
  name: daily-cluster-backup
  namespace: velero
spec:
  schedule: "0 1 * * *"          # Every night at 1:00 AM
  template:
    ttl: "720h"                  # Retain backups for 30 days
    includedNamespaces:
    - "*"
    excludedNamespaces:
    - kube-system
    - velero
    snapshotVolumes: true
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Velero Architecture and CSI VolumeSnapshot Integration](./03-Velero-Architecture-and-CSI-VolumeSnapshot-Integration.md) | [Index](../../../README.md) | [05 - Cross Cluster Migration and Cluster Rebuilding →](./05-Cross-Cluster-Migration-and-Cluster-Rebuilding.md) |
