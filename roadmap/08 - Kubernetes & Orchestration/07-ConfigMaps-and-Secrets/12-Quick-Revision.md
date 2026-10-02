# 12 - Quick-Revision & Enterprise Cheat Sheet

## ConfigMap & Secret Summary

- **Encoding vs Encryption:** Base64 is encoding; KMS envelope encryption is required for etcd rest security.
- **Volume Mounts:** Update dynamically via atomic symlinks (`..data`).
- **Env Variables:** Stale until pod restarts (use Stakater Reloader).
- **Scale Optimization:** `immutable: true`.
- **GitOps Tooling:** External Secrets Operator (ESO) + HashiCorp Vault / Cloud KMS.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (08-Storage-PV-PVC-and-StorageClasses) →](../08-Storage-PV-PVC-and-StorageClasses/01-Kubernetes-Storage-Architecture-PV-and-PVC-Binding.md) |
