# 04 - Kubernetes Ingress and Gateway API with Traefik

## 1. Traefik `IngressRoute` Custom Resource Definition (CRD)

While Traefik fully supports standard Kubernetes `Ingress` resources, standard Ingress lacks support for advanced routing, middlewares, and TCP/UDP routing. Traefik provides the **`IngressRoute`** CRD:

```yaml
apiVersion: traefik.io/v1alpha1
kind: IngressRoute
metadata:
  name: api-ingress
  namespace: production
spec:
  entryPoints:
    - websecure
  routes:
    - match: Host(`api.company.com`) && PathPrefix(`/v2`)
      kind: Rule
      services:
        - name: api-service-v2
          port: 8080
          weight: 80 # Canary Routing: 80% to V2
        - name: api-service-v1
          port: 8080
          weight: 20 # Canary Routing: 20% to V1
      middlewares:
        - name: rate-limit-middleware
  tls:
    secretName: company-tls-cert
```

---

## 2. Declaring Middleware as a CRD

```yaml
apiVersion: traefik.io/v1alpha1
kind: Middleware
metadata:
  name: rate-limit-middleware
  namespace: production
spec:
  rateLimit:
    average: 100
    burst: 50
    period: 1s
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Docker and Docker Compose Integration](./03-Docker-and-Docker-Compose-Integration.md) | [Index](../../../README.md) | [05 - Automated Lets Encrypt TLS Management →](./05-Automated-Lets-Encrypt-TLS-Management.md) |
