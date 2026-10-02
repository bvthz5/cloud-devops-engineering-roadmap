# 03 - Pod Affinity and Pod Anti-Affinity: Co-location Rules

## 1. High-Availability Anti-Affinity

To guarantee that no two replicas of an API pod are scheduled on the same physical worker node:

```yaml
spec:
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: app
            operator: In
            values: ["api"]
        topologyKey: "kubernetes.io/hostname"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Node Selection & Affinity](./02-Node-Selection-NodeName-NodeSelector-and-NodeAffinity.md) | [README](./README.md) | [04 - Taints & Tolerations](./04-Taints-and-Tolerations-Node-Cordon-and-Drain-Mechanics.md) |
