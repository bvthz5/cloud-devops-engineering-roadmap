# 12 - Quick-Revision & Enterprise Cheat Sheet

## Service DNS Anatomy

Format: `<service>.<namespace>.svc.<cluster-domain>`
Example: `order-service.production.svc.cluster.local`

- **ClusterIP:** Default internal VIP.
- **NodePort:** 30000–32767 on all nodes.
- **LoadBalancer:** Cloud-managed external L4 proxy.
- **Headless:** `clusterIP: None` (returns direct Pod IPs).
- **CoreDNS IP:** Usually `10.96.0.10` or `.10` of service subnet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 06 - Ingress Controllers](../06-Ingress-Controllers-and-Routing/README.md) |
