# 10 - Hands-On Practice Labs

## Lab 1: Modern Caddyfile with Microservice Reverse Proxy and Gzip

### Objective
Configure Caddy to proxy requests for `api.localhost` to a local backend with automatic response compression and custom security headers.

### Implementation (`Caddyfile`)
```caddyfile
api.localhost {
    encode gzip zstd

    header {
        X-Content-Type-Options nosniff
        X-Frame-Options DENY
    }

    reverse_proxy 127.0.0.1:8080 {
        header_up Host {host}
        header_up X-Real-IP {remote_host}
    }
}
```

```bash
# Run Caddy in foreground
caddy run --config Caddyfile
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
