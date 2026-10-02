# 10 - Hands-On Practice Labs

## Lab: Configuring Canary Traffic Splitting with HTTPRoute

```yaml
cat << 'EOF' > canary-route.yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: payment-split
spec:
  parentRefs:
  - name: internal-gateway
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /pay
    backendRefs:
    - name: payment-stable
      port: 80
      weight: 80
    - name: payment-canary
      port: 80
      weight: 20
EOF
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
