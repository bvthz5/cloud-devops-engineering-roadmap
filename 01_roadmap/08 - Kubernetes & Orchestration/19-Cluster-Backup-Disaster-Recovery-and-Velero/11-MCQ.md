# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What is the difference between etcd snapshot backups and Velero backups?
- [ ] A) etcd backups only back up secrets
- [x] B) etcd backs up control plane state; Velero backs up both cluster manifests and persistent storage volumes via CSI
- [ ] C) Velero cannot back up Custom Resource Definitions
- [ ] D) etcd snapshots can be stored directly in AWS S3 natively

<details>
<summary>Explanation</summary>
etcd snapshot saves the raw control plane key-value data in etcd. Velero acts at the Kubernetes API level, serializing resource manifests to object storage while orchestrating CSI volume snapshots for application disks.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
