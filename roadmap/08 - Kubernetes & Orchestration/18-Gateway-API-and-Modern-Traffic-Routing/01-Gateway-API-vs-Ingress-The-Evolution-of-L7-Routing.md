# 01 - Gateway API vs Ingress: The Evolution of L7 Routing

## 1. Why Was Ingress Replaced?

The original Kubernetes `Ingress` resource (created in 2015) had fundamental design flaws:
1. **Monolithic:** Merged infrastructure provisioning (ports, TLS) with application routing paths in a single resource.
2. **Annotation Proliferation:** Lacked native support for rewrites, canary, or rate limiting, leading to non-portable vendor annotations (`nginx.ingress.kubernetes.io/*`).
3. **No Cross-Namespace Routing:** An Ingress in `namespace-a` could not route traffic to a Service in `namespace-b`.

---

## 2. The Gateway API Standard (GA in k8s 1.25+)

Gateway API is expressive, extensible, and role-oriented:

```text
ROLE: INFRASTRUCTURE PROVIDER
[ GatewayClass ] (e.g., Envoy, Cilium, AWS VPC Lattice)
       │
       ▼
ROLE: CLUSTER OPERATOR
[ Gateway ] (Defines IP, Ports, TLS certificates, listeners)
       │
       ├── Attached via Route Rules
       ▼
ROLE: APPLICATION DEVELOPER
[ HTTPRoute / GRPCRoute / TCPRoute ] (Defines paths, headers, traffic weights)
       │
       ▼
[ Backend Services & Pods ]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (17-Advanced-Scheduling-Taints-Tolerations-and-Affinity)](../17-Advanced-Scheduling-Taints-Tolerations-and-Affinity/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Gateway API Role Oriented Architecture GatewayClass Gateway Route →](./02-Gateway-API-Role-Oriented-Architecture-GatewayClass-Gateway-Route.md) |
