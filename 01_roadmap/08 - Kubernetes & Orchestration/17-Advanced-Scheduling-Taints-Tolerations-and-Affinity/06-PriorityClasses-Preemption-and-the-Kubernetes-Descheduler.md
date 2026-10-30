# 06 - PriorityClasses, Preemption, and the Kubernetes Descheduler

## 1. Workload Priority and Preemption

When a cluster runs out of compute capacity, high-priority pods can **preempt (evict)** lower-priority pods:

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: mission-critical
value: 1000000
preemptionPolicy: PreemptLowerPriority
globalDefault: false
description: "Used for core transaction processing services."
```

---

## 2. The Kubernetes Descheduler

While `kube-scheduler` makes decisions when pods are created, node utilization changes over time. The **Descheduler** evicts pods that violate rules after initial placement (e.g. overutilized nodes, violated anti-affinity).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Topology Spread Constraints Multi AZ High Availability](./05-Topology-Spread-Constraints-Multi-AZ-High-Availability.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
