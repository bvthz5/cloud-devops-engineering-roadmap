# 09 - Interview Questions & Architectural Scenarios

### Q1: What happens if a NetworkPolicy is applied, but the cluster CNI does not support NetworkPolicies?
**Answer:**
Kubernetes API server will successfully accept and store the `NetworkPolicy` resource in `etcd`, but the network plugin (like basic Flannel) will simply ignore it! All pods will remain completely unisolated. You must run a policy-capable CNI (like Calico, Cilium, or Antrea) to enforce policies.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
