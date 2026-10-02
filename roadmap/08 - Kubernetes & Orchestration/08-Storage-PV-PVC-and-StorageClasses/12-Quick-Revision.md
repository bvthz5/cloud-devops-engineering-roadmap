# 12 - Quick-Revision & Enterprise Cheat Sheet

## Storage Cheat Sheet

- **RWO:** Single Node Read/Write (EBS, GPD).
- **RWX:** Multi Node Read/Write (EFS, NFS).
- **`volumeBindingMode: WaitForFirstConsumer`:** Mandatory for multi-AZ clusters.
- **Reclaim Policies:** `Delete` (data erased) vs `Retain` (data preserved).
- **CSI Drivers:** External Controller (Attach/Detach) + Node DaemonSet (Mount/Format).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 09 - StatefulSets & DaemonSets](../09-StatefulSets-and-DaemonSets/README.md) |
