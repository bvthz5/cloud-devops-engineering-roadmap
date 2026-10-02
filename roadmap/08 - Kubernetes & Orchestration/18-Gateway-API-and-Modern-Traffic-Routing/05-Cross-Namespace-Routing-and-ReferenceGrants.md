# 05 - Cross-Namespace Routing and ReferenceGrants

## 1. The Cross-Namespace Security Problem

If a Gateway in `infra` can route to Services in `payments`, what prevents an untrusted tenant in `sandbox` from attaching their route to the production Gateway or reading secrets from `payments`?

---

## 2. The ReferenceGrant Handshake

**ReferenceGrant** is an explicit authorization handshake. The owner of the resource must grant permission to the external Gateway:

```yaml
apiVersion: gateway.networking.k8s.io/v1beta1
kind: ReferenceGrant
metadata:
  name: allow-gateway-to-payments
  namespace: payments
spec:
  from:
  - group: gateway.networking.k8s.io
    kind: Gateway
    namespace: ingress-gateway
  to:
  - group: ""
    kind: Service
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Canary & Mirroring](./04-Canary-Traffic-Splitting-and-Mirroring-with-HTTPRoute.md) | [README](./README.md) | [06 - Gateway Implementations](./06-Gateway-API-Implementations-Envoy-Gateway-Cilium-Traefik.md) |
