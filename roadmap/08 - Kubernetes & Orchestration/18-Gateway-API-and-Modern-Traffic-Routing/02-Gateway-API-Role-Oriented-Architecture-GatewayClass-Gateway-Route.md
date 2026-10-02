# 02 - Gateway API Role-Oriented Architecture: GatewayClass, Gateway, and Route

## 1. The Gateway Resource Manifest

The Cluster Operator defines the entry point:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: prod-gateway
  namespace: ingress-gateway
spec:
  gatewayClassName: eg          # Envoy Gateway
  listeners:
  - name: https
    protocol: HTTPS
    port: 443
    tls:
      mode: Terminate
      certificateRefs:
      - name: prod-tls-secret
    allowedRoutes:
      namespaces:
        from: All               # Allows routes from any namespace!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Gateway API vs Ingress](./01-Gateway-API-vs-Ingress-The-Evolution-of-L7-Routing.md) | [README](./README.md) | [03 - HTTPRoute Syntax](./03-HTTPRoute-Advanced-Header-Matching-and-Path-Rewrites.md) |
