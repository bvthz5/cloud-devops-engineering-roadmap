# 06 - Caddy as a Kubernetes Ingress and Container Edge

## 1. Custom Binary Compilation with `xcaddy`

Because Go compiles into a single static binary, adding third-party plugins (like Cloudflare DNS for ACME DNS-01 challenges) is done using **`xcaddy`**:

```bash
# Install xcaddy
go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest

# Build Caddy with Cloudflare DNS plugin
xcaddy build \
  --with github.com/caddy-dns/cloudflare

# The resulting ./caddy binary now supports automated DNS-01 challenges!
```

---

## 2. Automated Wildcard Certificates with Cloudflare DNS

```caddyfile
*.example.com {
    tls {
        dns cloudflare {env.CLOUDFLARE_API_TOKEN}
    }

    @api host api.example.com
    handle @api {
        reverse_proxy localhost:8080
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Dynamic JSON API and Zero Downtime Config](./05-Dynamic-JSON-API-and-Zero-Downtime-Config.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
