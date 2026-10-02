# 02 - StorageClasses and Dynamic Provisioning

## 1. What Is a StorageClass?

A **StorageClass** defines the "profile" or tier of storage (e.g., fast SSD vs slow HDD) and the dynamic provisioner responsible for creating the volume on demand.

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-nvme
provisioner: ebs.csi.aws.com
volumeBindingMode: WaitForFirstConsumer    # CRITICAL FOR MULTI-AZ!
allowVolumeExpansion: true
reclaimPolicy: Delete
parameters:
  type: gp3
  iops: "3000"
  throughput: "125"
```

---

## 2. Why `WaitForFirstConsumer` Is Critical

- **`Immediate` (Default):** The volume is created as soon as the PVC is submitted. The storage provider might place the EBS disk in Availability Zone `us-east-1a`. If the pod is later scheduled to a node in `us-east-1b`, the disk cannot be mounted!
- **`WaitForFirstConsumer`:** Delays volume creation until the Pod is actually scheduled to a node. The volume is created in the exact same AZ where the Pod is placed!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Kubernetes Storage Architecture PV and PVC Binding](./01-Kubernetes-Storage-Architecture-PV-and-PVC-Binding.md) | [Index](../../../README.md) | [03 - Container Storage Interface CSI Architecture →](./03-Container-Storage-Interface-CSI-Architecture.md) |
