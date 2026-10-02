# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Pod Stuck in "Pending"

```text
[ Symptom: Pod remains in "Pending" state ]
                         │
                         ▼
        Run: kubectl describe pod <pod-name>
        Look at "Events" -> FailedScheduling:
        ├── "0/8 nodes are available: 8 Insufficient cpu" ──► Scale cluster nodes!
        ├── "node(s) had untolerated taint"               ──► Add toleration or check node taints.
        ├── "node(s) didn't match PodTopologySpread"      ──► Relax maxSkew or add nodes to underrepresented zone.
        └── "node(s) didn't match PodAntiAffinity"       ──► Not enough unique nodes to satisfy anti-affinity!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
