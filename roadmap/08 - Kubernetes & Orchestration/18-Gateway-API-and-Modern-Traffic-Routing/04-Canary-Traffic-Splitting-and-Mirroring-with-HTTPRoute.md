# 04 - Canary Traffic Splitting and Mirroring with HTTPRoute

## 1. Weighted Canary Routing

```yaml
spec:
  rules:
  - backendRefs:
    - name: payment-v1
      port: 80
      weight: 90               # 90% of traffic
    - name: payment-v2
      port: 80
      weight: 10               # 10% of traffic
```

---

## 2. Live Request Shadowing (Mirroring)

Send 100% of live traffic to production while duplicating (shadowing) a copy to test experimental code without impacting users:

```yaml
spec:
  rules:
  - filters:
    - type: RequestMirror
      requestMirror:
        backendRef:
          name: payment-shadow-tester
          port: 80
    backendRefs:
    - name: payment-prod
      port: 80
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - HTTPRoute Advanced Header Matching and Path Rewrites](./03-HTTPRoute-Advanced-Header-Matching-and-Path-Rewrites.md) | [Index](../../../README.md) | [05 - Cross Namespace Routing and ReferenceGrants →](./05-Cross-Namespace-Routing-and-ReferenceGrants.md) |
