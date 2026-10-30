# 03 - SSL/TLS Termination and Cert-Manager Integration

## 1. Automated PKI with Cert-Manager

**cert-manager** is the cloud-native X.509 certificate management controller for Kubernetes. It automatically provisions and renews TLS certificates from Let's Encrypt, HashiCorp Vault, or private enterprise CAs.

```text
Ingress (with cert-manager annotation)
   │
   ▼
cert-manager Controller detects Ingress
   ├── Creates Certificate & CertificateRequest CRDs
   ├── Performs ACME HTTP-01 challenge (verifies domain control)
   ├── Obtains signed X.509 certificate from Let's Encrypt
   └── Writes cert and private key to Kubernetes Secret: "api-tls-cert"
```

---

## 2. Production ClusterIssuer Manifest

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: security@my-org.com
    privateKeySecretRef:
      name: letsencrypt-prod-account-key
    solvers:
    - http01:
        ingress:
          class: nginx
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Ingress Nginx Architecture and Controller Mechanics](./02-Ingress-Nginx-Architecture-and-Controller-Mechanics.md) | [Index](../../../README.md) | [04 - Traffic Splitting and Canary Ingress Annotations →](./04-Traffic-Splitting-and-Canary-Ingress-Annotations.md) |
