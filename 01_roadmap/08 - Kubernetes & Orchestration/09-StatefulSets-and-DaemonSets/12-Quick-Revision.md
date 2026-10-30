# 12 - Quick-Revision & Enterprise Cheat Sheet

## StatefulSet vs DaemonSet Summary

- **StatefulSet:** Stable ordinals (`app-0`), dedicated PVC per replica (`volumeClaimTemplates`), headless service required, scaled down PVCs are preserved.
- **DaemonSet:** Exactly 1 pod per node, auto-scales with node count, tolerates master taints with `operator: Exists`.
- **Scaling Order:** OrderedReady starts $0 	o N-1$, terminates $N-1 	o 0$.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (10-Jobs-and-CronJobs) →](../10-Jobs-and-CronJobs/01-Kubernetes-Job-Controller-and-Completion-Guarantees.md) |
