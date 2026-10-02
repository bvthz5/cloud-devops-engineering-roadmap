# 06 - Autoscaling Anti-Patterns, Thrashing, and Conflict Resolution

## 1. Flapping (Thrashing)

**Flapping** occurs when an application scales up due to traffic, CPU drops immediately, HPA scales down, CPU spikes again, and HPA scales back up in an endless thrashing cycle.

### Defense: Stabilization Windows
```yaml
behavior:
  scaleDown:
    stabilizationWindowSeconds: 300     # Wait 5 minutes before applying scale-down decisions!
```

---

## 2. Missing Resource Requests

If `resources.requests.cpu` is omitted on a Pod, **HPA cannot calculate percentage utilization!**
The HPA status will display: `TARGETS: <unknown>/50%` and will fail to scale.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Karpenter Autoscaling](./05-Karpenter-Next-Generation-Just-in-Time-Node-Autoscaling.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
