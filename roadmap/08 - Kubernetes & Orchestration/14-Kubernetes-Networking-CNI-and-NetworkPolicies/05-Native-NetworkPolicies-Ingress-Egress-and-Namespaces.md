# 05 - Native NetworkPolicies: Ingress, Egress, and Namespaces

## 1. Default Behavior: Flat Open Network

By default, **Kubernetes networks are non-isolated.** Any pod in any namespace can ping and send traffic to any other pod across the entire cluster!

---

## 2. Production Zero-Trust Pattern: Default Deny All

```yaml
# 1. Deny ALL incoming and outgoing traffic in the namespace
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: secure-workloads
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
---
# 2. Explicitly permit database ingress ONLY from frontend pods
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-db
  namespace: secure-workloads
spec:
  podSelector:
    matchLabels:
      app: postgres
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 5432
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Cilium CNI eBPF Datapath and High Performance Networking](./04-Cilium-CNI-eBPF-Datapath-and-High-Performance-Networking.md) | [Index](../../../README.md) | [06 - Advanced L7 Network Policies and DNS Filtering →](./06-Advanced-L7-Network-Policies-and-DNS-Filtering.md) |
