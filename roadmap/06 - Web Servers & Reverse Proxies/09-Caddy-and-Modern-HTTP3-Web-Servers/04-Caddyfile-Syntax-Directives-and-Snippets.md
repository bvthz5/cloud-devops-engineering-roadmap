# 04 - Caddyfile Syntax, Directives, and Snippets

## 1. Production Caddyfile with Matchers and Snippets

```caddyfile
# Reusable Security Snippet
(security_headers) {
    header {
        Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        X-Content-Type-Options "nosniff"
        X-Frame-Options "DENY"
        Referrer-Policy "strict-origin-when-cross-origin"
    }
}

api.example.com {
    import security_headers

    # Named Matcher: Only match /v1/auth path
    @auth_routes path /v1/auth/*

    # Reverse proxy with load balancing and health checks
    reverse_proxy @auth_routes 10.0.1.10:8000 10.0.1.11:8000 {
        lb_policy least_conn
        health_uri /healthz
        health_interval 5s
    }

    # Catch-all route to main service
    reverse_proxy 10.0.2.10:8080

    # Structured JSON Logging
    log {
        output file /var/log/caddy/access.log
        format json
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Native HTTP3 QUIC and 0 RTT](./03-Native-HTTP3-QUIC-and-0-RTT.md) | [Index](../../../README.md) | [05 - Dynamic JSON API and Zero Downtime Config →](./05-Dynamic-JSON-API-and-Zero-Downtime-Config.md) |
