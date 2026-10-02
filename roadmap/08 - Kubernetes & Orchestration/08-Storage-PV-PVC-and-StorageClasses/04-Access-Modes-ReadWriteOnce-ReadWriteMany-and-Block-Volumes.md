# 04 - Access Modes: ReadWriteOnce, ReadWriteMany, and Block Volumes

## 1. Storage Access Modes

| Mode | Acronym | Description | Example Storage |
|---|:---:|---|---|
| **ReadWriteOnce** | `RWO` | Mounted as read-write by a **single node**. | AWS EBS, Azure Disk, GCP Persistent Disk |
| **ReadOnlyMany** | `ROX` | Mounted read-only by **many nodes** simultaneously. | ISO images, read-only NFS |
| **ReadWriteMany** | `RWX` | Mounted as read-write by **many nodes** simultaneously. | AWS EFS, NFS, CephFS, GlusterFS |
| **ReadWriteOncePod** | `RWOP` | Mounted as read-write by a **single Pod** (k8s 1.22+). | CSI strict single-tenant volume |

> [!WARNING]
> You cannot mount standard cloud block storage (EBS / GPD) across multiple pods running on different worker nodes! Attempting to do so causes the dreaded `Multi-Attach error`. Use EFS/NFS for `RWX`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - CSI Architecture](./03-Container-Storage-Interface-CSI-Architecture.md) | [README](./README.md) | [05 - Volume Expansion](./05-Volume-Expansion-and-Reclaim-Policies.md) |
