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
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (09-StatefulSets-and-DaemonSets) →](../09-StatefulSets-and-DaemonSets/01-StatefulSet-Architecture-Stable-Network-and-Storage-Identity.md) |
