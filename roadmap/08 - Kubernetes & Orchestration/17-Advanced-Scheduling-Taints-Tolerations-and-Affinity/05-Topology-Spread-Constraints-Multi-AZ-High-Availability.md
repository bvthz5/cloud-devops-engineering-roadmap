# 05 - Topology Spread Constraints: Multi-AZ High Availability

## 1. Even Workload Distribution

**Topology Spread Constraints** ensure pods are distributed evenly across Availability Zones, regions, or racks to prevent concentration in a single failure domain.

```yaml
spec:
  topologySpreadConstraints:
  - maxSkew: 1                  # Max difference in pod count between any two zones
    topologyKey: topology.kubernetes.io/zone
    whenUnsatisfiable: DoNotSchedule
    labelSelector:
      matchLabels:
        app: web
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Taints and Tolerations Node Cordon and Drain Mechanics](./04-Taints-and-Tolerations-Node-Cordon-and-Drain-Mechanics.md) | [Index](../../../README.md) | [06 - PriorityClasses Preemption and the Kubernetes Descheduler →](./06-PriorityClasses-Preemption-and-the-Kubernetes-Descheduler.md) |
