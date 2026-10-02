# 12 - Quick-Revision & Enterprise Cheat Sheet

```caddyfile
# Caddyfile Golden Standard Template
example.com {
    encode gzip zstd
    header Strict-Transport-Security "max-age=31536000"

    reverse_proxy 127.0.0.1:8080 {
        health_uri /healthz
        health_interval 5s
    }
}
```

```bash
# Common Caddy CLI Commands
caddy run --config Caddyfile
caddy reload --config Caddyfile
caddy fmt --overwrite
caddy validate
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [10 - Web Security, WAF & Hardening](../10-Web-Security-WAF-and-Hardening/README.md) |
