# 01 - Kubernetes Scheduler Architecture and Scoring Phases

## 1. The Scheduling Framework

The `kube-scheduler` assigns unscheduled pods to nodes through two main steps:
1. **Filtering (Predicates):** Eliminates nodes that do not satisfy requirements (ports, CPU/RAM capacity, taints).
2. **Scoring (Priorities):** Ranks the surviving nodes with a score from 0 to 100, picking the highest scorer.

```text
Unscheduled Pod ──► [ 1. PreFilter / Filter (Predicates) ] ──► Candidate Nodes
                                     │
                                     ▼
                     [ 2. PreScore / Score (Priorities) ]  ──► Ranked Node List
                                     │
                                     ▼
                     [ 3. Reserve / Permit / Bind ]       ──► Writes Binding to API Server
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Node Selection & Affinity](./02-Node-Selection-NodeName-NodeSelector-and-NodeAffinity.md) |
