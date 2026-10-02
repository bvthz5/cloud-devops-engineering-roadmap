# 12 - Quick-Revision & Enterprise Cheat Sheet

## Autoscaling Summary

- **HPA:** Scales pod count horizontally based on metrics (CPU, RAM, SQS, QPS).
- **VPA:** Resizes pod requests vertically (CPU, RAM).
- **Cluster Autoscaler:** Adds/removes nodes based on `Pending` pods and node underutilization.
- **Karpenter:** Rapid, just-in-time node provisioning directly via cloud compute APIs.
- **Flapping Prevention:** `stabilizationWindowSeconds: 300`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 12 - RBAC & Security](../12-RBAC-and-Cluster-Security/README.md) |
