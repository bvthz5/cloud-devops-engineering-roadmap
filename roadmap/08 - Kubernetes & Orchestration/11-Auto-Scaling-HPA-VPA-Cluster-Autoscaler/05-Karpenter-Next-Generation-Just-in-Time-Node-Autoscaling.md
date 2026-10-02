# 05 - Karpenter: Next-Generation Just-in-Time Node Autoscaling

## 1. Why Karpenter Replaces Cluster Autoscaler

Traditional Cluster Autoscaler relies on rigid cloud Auto Scaling Groups (ASGs).
**Karpenter** (open-source, CNCF) bypasses ASGs completely, calling cloud compute APIs directly:
- **Speed:** Provisions right-sized nodes in **30-45 seconds** (vs 4-7 minutes with ASG).
- **Flexible Bin-Packing:** Analyzes pending pod shapes and launches the exact instance type needed (e.g. 1x `c6i.4xlarge` instead of 4x `t3.medium`).
- **Native Spot Disruption:** Gracefully drains spot instances when AWS sends a 2-minute preemption notice.

```yaml
apiVersion: karpenter.sh/v1beta1
kind: NodePool
metadata:
  name: general-compute
spec:
  template:
    spec:
      requirements:
      - key: karpenter.sh/capacity-type
        operator: In
        values: ["spot", "on-demand"]
      - key: kubernetes.io/arch
        operator: In
        values: ["amd64", "arm64"]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Cluster Autoscaler Architecture and Cloud Integration](./04-Cluster-Autoscaler-Architecture-and-Cloud-Integration.md) | [Index](../../../README.md) | [06 - Autoscaling Anti Patterns Thrashing and Conflict Resolution →](./06-Autoscaling-Anti-Patterns-Thrashing-and-Conflict-Resolution.md) |
