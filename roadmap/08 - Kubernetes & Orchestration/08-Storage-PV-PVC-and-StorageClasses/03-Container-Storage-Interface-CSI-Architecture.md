# 03 - Container Storage Interface (CSI) Architecture

## 1. How CSI Works

The **Container Storage Interface (CSI)** is an industry-standard gRPC specification that enables third-party storage vendors (AWS, NetApp, Ceph, Pure Storage) to write plugins without modifying Kubernetes core source code.

```text
[ Kubernetes API Server ]
          │
          ▼
[ CSI Controller (Deployment) ]
  ├── external-provisioner (Calls CreateVolume on Cloud API)
  ├── external-attacher (Calls AttachVolume to EC2 instance)
  └── external-resizer (Calls ExpandVolume)
          │
          ▼
[ CSI Node Plugin (DaemonSet on every worker) ]
  ├── NodeStageVolume (Formats filesystem: mkfs.ext4)
  └── NodePublishVolume (Mounts to pod directory: /var/lib/kubelet/pods/...)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - StorageClasses and Dynamic Provisioning](./02-StorageClasses-and-Dynamic-Provisioning.md) | [Index](../../../README.md) | [04 - Access Modes ReadWriteOnce ReadWriteMany and Block Volumes →](./04-Access-Modes-ReadWriteOnce-ReadWriteMany-and-Block-Volumes.md) |
