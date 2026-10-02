# 04 - Cluster Autoscaler Architecture and Cloud Integration

## 1. How Cluster Autoscaler Works

The **Cluster Autoscaler** adjusts the size of the worker node pool when:
1. **Scale Up:** Pods are in `Pending` state because no existing node has sufficient allocatable CPU/RAM.
2. **Scale Down:** Nodes have been consistently underutilized (< 50% requested capacity) for a sustained period (default 10 minutes), and their pods can be accommodated on other nodes.

```text
[ HPA scales pods 4 ──► 20 ]
             │
             ▼
[ Pods 12 to 20 enter "Pending" state ]
             │
             ▼
[ Cluster Autoscaler detects Pending Pods ]
  ├── Simulates scheduling on candidate node groups
  └── Calls Cloud Provider API (AWS ASG / Azure VMSS / GCP MIG)
             │
             ▼
[ 3 New VMs Booted & Join Cluster ──► Pending Pods Scheduled! ]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Vertical Pod Autoscaler](./03-Vertical-Pod-Autoscaler-VPA-Modes-and-Recommender.md) | [README](./README.md) | [05 - Karpenter Autoscaling](./05-Karpenter-Next-Generation-Just-in-Time-Node-Autoscaling.md) |
