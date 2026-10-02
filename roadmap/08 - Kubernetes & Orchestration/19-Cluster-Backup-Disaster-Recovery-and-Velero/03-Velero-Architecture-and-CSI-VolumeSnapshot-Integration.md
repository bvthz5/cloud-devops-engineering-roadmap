# 03 - Velero Architecture and CSI VolumeSnapshot Integration

## 1. How Velero Works

While etcd backups capture raw cluster state, they do not back up persistent storage volumes (EBS, Persistent Disks).
**Velero** backs up:
1. **Cluster Metadata:** Serialized JSON manifests stored in an S3/GCS bucket.
2. **Persistent Volumes:** Invokes cloud storage snapshots via CSI VolumeSnapshot API or copies data via Restic/Kopia.

```text
[ Velero CLI: velero backup create prod-backup ]
                        │
                        ▼
         [ Velero Server Controller ]
           ├── Queries API Server: Exports manifests to S3
           └── Calls CSI Plugin: Takes snapshots of all PVs
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Etcd Backup & Restore](./02-Etcd-Backup-Snapshot-Save-and-Disaster-Restore.md) | [README](./README.md) | [04 - Scheduled Automated Backups](./04-Scheduled-Automated-Backups-and-Object-Storage-Targets.md) |
