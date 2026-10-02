# 02 - Node Selection: NodeName, NodeSelector, and NodeAffinity

## 1. NodeAffinity: Hard vs Soft Rules

- **`requiredDuringSchedulingIgnoredDuringExecution` (Hard):** Pod **will not schedule** unless the node matches this rule.
- **`preferredDuringSchedulingIgnoredDuringExecution` (Soft):** Scheduler **attempts** to place the pod on matching nodes, but falls back to other nodes if unavailable.

```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In
            values: ["us-east-1a", "us-east-1b"]
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 80
        preference:
          matchExpressions:
          - key: disktype
            operator: In
            values: ["ssd"]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Scheduler Architecture](./01-Kubernetes-Scheduler-Architecture-and-Scoring-Phases.md) | [README](./README.md) | [03 - Pod Affinity & Anti-Affinity](./03-Pod-Affinity-and-Pod-Anti-Affinity-Co-location-Rules.md) |
