# 06 - Ingress Security: Rate Limiting and ModSecurity WAF

## 1. Edge Rate Limiting

To prevent Layer 7 denial of service (DoS) and brute force attacks:

```yaml
metadata:
  annotations:
    # Limit requests per second per IP
    nginx.ingress.kubernetes.io/limit-rps: "20"
    # Allow burst of 50 requests
    nginx.ingress.kubernetes.io/limit-burst-multiplier: "5"
    # Limit concurrent connections per IP
    nginx.ingress.kubernetes.io/limit-connections: "10"
```

---

## 2. Enabling ModSecurity & OWASP CRS

```yaml
metadata:
  annotations:
    nginx.ingress.kubernetes.io/enable-modsecurity: "true"
    nginx.ingress.kubernetes.io/enable-owasp-core-rules: "true"
    nginx.ingress.kubernetes.io/modsecurity-snippet: |
      SecRuleEngine On
      SecRequestBodyAccess On
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Rewrite Target Custom Headers and CORS Policies](./05-Rewrite-Target-Custom-Headers-and-CORS-Policies.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
