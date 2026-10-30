# 12 - Quick-Revision & Enterprise Cheat Sheet

## Gateway API Summary

- **Three Roles:** `GatewayClass` (Vendor) ➔ `Gateway` (Operator) ➔ `HTTPRoute` (Developer).
- **No Annotations:** Rewrites, Canary weights, and Header matching are native YAML fields.
- **Cross-Namespace Security:** Explicit handshake via `ReferenceGrant`.
- **Implementations:** Envoy Gateway, Cilium, Istio.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (19-Cluster-Backup-Disaster-Recovery-and-Velero) →](../19-Cluster-Backup-Disaster-Recovery-and-Velero/01-Kubernetes-Disaster-Recovery-Strategy-RTO-and-RPO.md) |
