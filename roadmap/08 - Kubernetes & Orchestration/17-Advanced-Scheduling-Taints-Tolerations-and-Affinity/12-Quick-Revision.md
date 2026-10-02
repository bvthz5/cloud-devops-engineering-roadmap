# 12 - Quick-Revision & Enterprise Cheat Sheet

## Scheduling Cheat Sheet

- **NodeAffinity:** Hard (`required...`) vs Soft (`preferred...`).
- **PodAntiAffinity:** Prevents co-locating pods on same `topologyKey: kubernetes.io/hostname`.
- **Taints & Tolerations:** Node repels workloads (`NoSchedule`, `NoExecute`).
- **TopologySpread:** Spreads across AZs with `maxSkew: 1`.
- **Maintenance:** `kubectl cordon` ➔ `kubectl drain`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 18 - Gateway API](../18-Gateway-API-and-Modern-Traffic-Routing/README.md) |
