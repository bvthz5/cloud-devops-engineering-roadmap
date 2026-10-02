# 04 - Taints and Tolerations: Node Cordon and Drain Mechanics

## 1. Taints Repel Workloads

While Affinity attracts pods to nodes, **Taints** repel pods unless the pod possesses a matching **Toleration**.

```text
Node with Taint: "gpu=true:NoSchedule"
  ├── Standard Pod (No toleration) ──────► REPELLED! (Cannot schedule)
  └── ML Training Pod (With toleration) ─► ADMITTED! (Can schedule)
```

### Taint Effects:
- **`NoSchedule`:** New pods without toleration will not schedule; running pods stay.
- **`PreferNoSchedule`:** Scheduler tries to avoid scheduling, but allows if no other node available.
- **`NoExecute`:** Running pods without toleration are **evicted immediately**!

---

## 2. Maintenance: Cordon and Drain

```bash
# 1. Mark node unschedulable (prevents new pods)
kubectl cordon worker-01

# 2. Evict running pods safely respecting PDBs
kubectl drain worker-01 --ignore-daemonsets --delete-emptydir-data

# 3. Uncordon after maintenance
kubectl uncordon worker-01
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Pod Affinity and Pod Anti Affinity Co location Rules](./03-Pod-Affinity-and-Pod-Anti-Affinity-Co-location-Rules.md) | [Index](../../../README.md) | [05 - Topology Spread Constraints Multi AZ High Availability →](./05-Topology-Spread-Constraints-Multi-AZ-High-Availability.md) |
