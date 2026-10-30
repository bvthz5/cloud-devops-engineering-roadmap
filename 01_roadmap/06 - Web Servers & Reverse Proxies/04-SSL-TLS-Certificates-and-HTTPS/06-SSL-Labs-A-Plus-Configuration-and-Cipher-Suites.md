# 06 - SSL Labs A+ Hardening and Cipher Suites

## 1. Production Hardened TLS Configuration

Achieving an **A+ rating on Qualys SSL Labs** requires disabling legacy protocols (SSLv3, TLS 1.0, TLS 1.1), enforcing strong forward-secrecy cipher suites, and injecting HSTS headers.

```nginx
server {
    listen 443 ssl http2;
    server_name example.com;

    # Certificate Bundle & Private Key
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # 1. Enforce Modern TLS Protocols Only
    ssl_protocols TLSv1.2 TLSv1.3;

    # 2. Strong Cipher Suites (ECDHE + AES-GCM / CHACHA20)
    ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305';
    ssl_prefer_server_ciphers off; # Let client pick fastest hardware-accelerated cipher

    # 3. HTTP Strict Transport Security (HSTS - 2 Years with Subdomains and Preload)
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;

    # 4. OCSP Stapling
    ssl_stapling on;
    ssl_stapling_verify on;
    ssl_trusted_certificate /etc/letsencrypt/live/example.com/chain.pem;
    resolver 1.1.1.1 8.8.8.8 valid=300s;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Mutual TLS mTLS Architecture and Zero Trust](./05-Mutual-TLS-mTLS-Architecture-and-Zero-Trust.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
