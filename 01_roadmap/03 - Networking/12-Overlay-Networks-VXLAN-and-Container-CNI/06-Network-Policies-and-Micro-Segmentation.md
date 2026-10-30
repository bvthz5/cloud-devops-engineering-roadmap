# 06 - NetworkPolicies and Microsegmentation

## 1. Default Behavior: Flat Open Network

By default in Kubernetes, **all Pods can communicate with all other Pods across all namespaces with zero restrictions**.

---

## 2. Kubernetes NetworkPolicy Spec: Deny All Inbound

Applying this manifest blocks all unauthorized lateral traffic into the `backend` namespace:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all-ingress
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

---

## 3. Advanced Layer 7 NetworkPolicy in Cilium

Standard Kubernetes NetworkPolicies only filter IP addresses and ports (L3/L4).
**Cilium allows filtering by DNS name and HTTP method/path (L7):**

```yaml
apiVersion: "cilium.io/v2"
kind: CiliumNetworkPolicy
metadata:
  name: allow-stripe-api-only
spec:
  endpointSelector:
    matchLabels:
      app: payment-service
  egress:
  - toFQDNs:
    - matchName: "api.stripe.com"
    toPorts:
    - ports:
      - port: "443"
        protocol: TCP
      rules:
        http:
        - method: "POST"
          path: "/v1/charges"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - MTU Calculations Overhead and Clamping in Overlays](./05-MTU-Calculations-Overhead-and-Clamping-in-Overlays.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
