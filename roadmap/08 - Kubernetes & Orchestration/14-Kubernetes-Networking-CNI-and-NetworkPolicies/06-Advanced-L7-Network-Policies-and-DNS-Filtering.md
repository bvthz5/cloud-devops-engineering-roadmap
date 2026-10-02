# 06 - Advanced L7 Network Policies and DNS Filtering

## 1. Why Native NetworkPolicies Are Insufficient

Native Kubernetes NetworkPolicies can only filter on Layer 3 (IPs/CIDRs) and Layer 4 (TCP/UDP ports).
They cannot:
- Filter egress traffic to external domain names (e.g. `api.github.com`).
- Enforce HTTP REST rules (e.g. Allow `GET /health`, Block `POST /admin`).

---

## 2. Cilium L7 Network Policy (CiliumNetworkPolicy)

```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: egress-fqdn-and-l7
  namespace: default
spec:
  endpointSelector:
    matchLabels:
      app: payment-worker
  egress:
  # 1. Whitelist exact FQDN (resolves dynamic IPs automatically!)
  - toFQDNs:
    - matchName: "api.stripe.com"
    toPorts:
    - ports:
      - port: "443"
        protocol: TCP
  # 2. Restrict internal microservice HTTP methods
  - toEndpoints:
    - matchLabels:
        app: user-service
    toPorts:
    - ports:
      - port: "8080"
        protocol: TCP
      rules:
        http:
        - method: "GET"
          path: "/users/.*"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Native NetworkPolicies](./05-Native-NetworkPolicies-Ingress-Egress-and-Namespaces.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
