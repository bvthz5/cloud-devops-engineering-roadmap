# 03 - Vertical Pod Autoscaler (VPA) Modes and Recommender

## 1. What Is VPA?

While HPA scales the *quantity* of pods, the **Vertical Pod Autoscaler (VPA)** adjusts the *CPU and memory requests and limits* of containers.

### VPA Modes:
- **`Off` (Safest):** Only computes recommended resource values without modifying pods. SREs inspect recommendations via `kubectl describe vpa`.
- **`Initial`:** Sets requests only when a pod is first created; never touches running pods.
- **`Auto` / `Recreate`:** Evicts and recreates running pods to apply updated resource boundaries.

> [!WARNING]
> **Never configure HPA and VPA on the same resource (e.g. CPU) simultaneously!** They will enter a feedback loop and fight each other. Use VPA for memory and HPA for CPU or custom metrics.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Custom Metrics](./02-Custom-and-External-Metrics-with-Prometheus-Adapter.md) | [README](./README.md) | [04 - Cluster Autoscaler](./04-Cluster-Autoscaler-Architecture-and-Cloud-Integration.md) |
