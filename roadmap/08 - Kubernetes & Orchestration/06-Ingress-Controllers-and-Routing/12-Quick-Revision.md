# 12 - Quick-Revision & Enterprise Cheat Sheet

## Ingress Critical Annotations

| Annotation | Purpose |
|---|---|
| `nginx.ingress.kubernetes.io/ssl-redirect: "true"` | Force HTTPS redirection |
| `nginx.ingress.kubernetes.io/rewrite-target: /$2` | Rewrite URL path |
| `nginx.ingress.kubernetes.io/canary: "true"` | Enable canary traffic splitting |
| `nginx.ingress.kubernetes.io/canary-weight: "15"` | Send 15% traffic to canary |
| `cert-manager.io/cluster-issuer: "letsencrypt-prod"`| Auto-issue TLS certificate |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (07-ConfigMaps-and-Secrets) →](../07-ConfigMaps-and-Secrets/01-ConfigMap-Creation-Environment-and-Volume-Projections.md) |
