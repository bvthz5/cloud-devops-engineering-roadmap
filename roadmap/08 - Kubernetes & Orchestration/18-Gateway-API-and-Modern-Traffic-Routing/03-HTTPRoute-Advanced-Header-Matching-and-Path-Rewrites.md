# 03 - HTTPRoute: Advanced Header Matching and Path Rewrites

## 1. Native Routing Without Annotations

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: user-service-route
  namespace: default
spec:
  parentRefs:
  - name: prod-gateway
    namespace: ingress-gateway
  hostnames:
  - "api.example.com"
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /v2/users
      headers:
      - name: X-Beta-User
        value: "true"
    filters:
    - type: URLRewrite
      urlRewrite:
        path:
          type: ReplacePrefixMatch
          replacePrefixMatch: /users
    backendRefs:
    - name: user-service-v2
      port: 8080
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Role-Oriented Design](./02-Gateway-API-Role-Oriented-Architecture-GatewayClass-Gateway-Route.md) | [README](./README.md) | [04 - Canary & Mirroring](./04-Canary-Traffic-Splitting-and-Mirroring-with-HTTPRoute.md) |
