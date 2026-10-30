# 05 - Rewrite Target, Custom Headers, and CORS Policies

## 1. URL Path Rewriting

When an external client requests `https://api.example.com/customers/orders`, but the internal backend service only listens at `/orders`, use `rewrite-target`:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: rewrite-ingress
  annotations:
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  ingressClassName: nginx
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /customers(/|$)(.*)
        pathType: ImplementationSpecific
        backend:
          service:
            name: customers-service
            port:
              number: 8080
```

---

## 2. Production CORS Policy

```yaml
metadata:
  annotations:
    nginx.ingress.kubernetes.io/enable-cors: "true"
    nginx.ingress.kubernetes.io/cors-allow-methods: "GET, POST, PUT, DELETE, OPTIONS"
    nginx.ingress.kubernetes.io/cors-allow-origin: "https://my-frontend.com"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Traffic Splitting and Canary Ingress Annotations](./04-Traffic-Splitting-and-Canary-Ingress-Annotations.md) | [Index](../../../README.md) | [06 - Ingress Security Rate Limiting and ModSecurity WAF →](./06-Ingress-Security-Rate-Limiting-and-ModSecurity-WAF.md) |
