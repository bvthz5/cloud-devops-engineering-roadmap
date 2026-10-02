# 02 - Automatic HTTPS and Internal PKI

## 1. Zero-Config Automatic HTTPS

In Caddy, if you specify a domain name in the configuration, Caddy **automatically**:
1. Generates cryptographic private keys.
2. Issues an ACME certificate from Let's Encrypt (with ZeroSSL as automatic fallback).
3. Configures HTTP-to-HTTPS 301 redirection on port 80.
4. Enables OCSP stapling and automated renewals 30 days before expiration.

```caddyfile
# That's it! Full HTTPS enabled with Let's Encrypt!
app.example.com {
    reverse_proxy localhost:8080
}
```

---

## 2. On-Demand TLS for Multi-Tenant SaaS

In multi-tenant SaaS platforms where thousands of customers point custom CNAME domains to your service, generating certificates ahead of time is impossible.

**On-Demand TLS** obtains certificates dynamically during the **TLS Server Name Indication (SNI) handshake**:
```caddyfile
{
    on_demand_tls {
        # Security Guard: Consult backend API to confirm customer is legitimate
        ask http://127.0.0.1:9000/validate-domain
    }
}

:443 {
    tls {
        on_demand
    }
    reverse_proxy localhost:8080
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Caddy Architecture](./01-Caddy-Architecture-and-The-Go-Runtime.md) | [README](./README.md) | [03 - Native HTTP/3, QUIC & 0-RTT](./03-Native-HTTP3-QUIC-and-0-RTT.md) |
