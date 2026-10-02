# 04 - Traffic Splitting and Canary Ingress Annotations

## 1. Canary Traffic Splitting

Ingress-Nginx provides native canary routing annotations that allow sending a percentage of production traffic to a new version without modifying DNS or external load balancers.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: payments-canary
  annotations:
    nginx.ingress.kubernetes.io/canary: "true"
    nginx.ingress.kubernetes.io/canary-weight: "10"       # 10% of traffic
spec:
  ingressClassName: nginx
  rules:
  - host: payments.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: payments-v2
            port:
              number: 80
```

---

## 2. Advanced Canary Matchers

- **Header-based:** `nginx.ingress.kubernetes.io/canary-by-header: "X-Beta-Tester"`
- **Cookie-based:** `nginx.ingress.kubernetes.io/canary-by-cookie: "user_group_beta"`

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - SSL TLS Termination and Cert Manager Integration](./03-SSL-TLS-Termination-and-Cert-Manager-Integration.md) | [Index](../../../README.md) | [05 - Rewrite Target Custom Headers and CORS Policies →](./05-Rewrite-Target-Custom-Headers-and-CORS-Policies.md) |
