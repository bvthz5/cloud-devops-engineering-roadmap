# 01 - Ingress Resource Specification and Path Routing

## 1. What Is Ingress?

An **Ingress** is an API object that manages external access to services in a cluster, typically HTTP/HTTPS. Ingress can provide load balancing, SSL termination, and name-based virtual hosting.

```text
    Internet
       │
       ▼
[ Ingress Controller (e.g., Ingress-Nginx) ]
  ├── Host: api.example.com
  │     └── Path: /v1  ──► Service: api-v1 (ClusterIP: 10.96.1.10)
  │     └── Path: /v2  ──► Service: api-v2 (ClusterIP: 10.96.1.20)
  └── Host: store.example.com
        └── Path: /    ──► Service: store-web (ClusterIP: 10.96.1.30)
```

---

## 2. Ingress Specification Manifest

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: main-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - api.example.com
    secretName: api-tls-cert
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /users
        pathType: Prefix
        backend:
          service:
            name: users-service
            port:
              number: 80
      - path: /orders
        pathType: Prefix
        backend:
          service:
            name: orders-service
            port:
              number: 80
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Ingress-Nginx Architecture](./02-Ingress-Nginx-Architecture-and-Controller-Mechanics.md) |
