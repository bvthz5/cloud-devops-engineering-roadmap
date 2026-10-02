# 05 - Automated Let's Encrypt TLS Management

## 1. Native ACME Engine in Traefik

Traefik contains a built-in ACME client that automatically requests, validates, installs, and renews TLS certificates from Let's Encrypt or ZeroSSL without requiring external tools like Certbot!

```yaml
# traefik.yml (Static Config)
certificatesResolvers:
  letsencrypt:
    acme:
      email: "admin@example.com"
      storage: "/etc/traefik/acme.json"
      # Use HTTP-01 challenge
      httpChallenge:
        entryPoint: "web"

entryPoints:
  web:
    address: ":80"
    # Auto-redirect HTTP to HTTPS globally
    http:
      redirections:
        entryPoint:
          to: "websecure"
          scheme: "https"
  websecure:
    address: ":443"
```

### Enforcing Permissions on `acme.json`
Traefik **refuses to start** if the certificate storage file is readable by other users:
```bash
touch /etc/traefik/acme.json
chmod 600 /etc/traefik/acme.json # Strict 0600 permissions mandatory!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Kubernetes Ingress & Gateway API](./04-Kubernetes-Ingress-and-Gateway-API-with-Traefik.md) | [README](./README.md) | [06 - Observability & Dashboard](./06-Observability-Tracing-and-Dashboard.md) |
