# 06 - Cluster Auditing: Event Analysis and Forensics

## 1. Inspecting Cluster Events

Events provide chronological forensics for scheduling, scaling, and crashes:

```bash
# View all recent events sorted by creation timestamp
kubectl get events --sort-by='.metadata.creationTimestamp' -A

# Filter only Warning and Error events
kubectl get events --field-selector type=Warning -A
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Ephemeral Debug Containers](./05-Ephemeral-Debug-Containers-and-Kubectl-Debug.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
