# 05 - Mutual TLS (mTLS) and Zero-Trust Architecture

## 1. Standard TLS vs Mutual TLS (mTLS)

```text
Standard One-Way TLS (Server Authentication Only):
Client ──► (Validates Server Certificate) ──► Server Authenticated
Server ──► (Has zero cryptographic identity of client; relies on API keys / passwords)

Mutual TLS / mTLS (Two-Way Cryptographic Authentication):
Client ──► (Validates Server Certificate) ──► Server Authenticated
Server ──► (Validates Client Certificate against internal CA) ──► Client Authenticated
* Cryptographic Zero-Trust: If client has no valid signed certificate, TCP handshake terminates!
```

---

## 2. Implementing mTLS in Nginx

```nginx
server {
    listen 443 ssl http2;
    server_name internal-api.corp.local;

    # Server TLS Credentials
    ssl_certificate /etc/nginx/ssl/server.crt;
    ssl_certificate_key /etc/nginx/ssl/server.key;

    # mTLS Client Verification
    ssl_client_certificate /etc/nginx/ssl/internal_ca.crt;
    ssl_verify_client on; # Enforce mandatory valid client certificate
    ssl_verify_depth 2;

    location / {
        # Pass verified client identity to upstream application
        proxy_pass http://internal_service;
        proxy_set_header X-Client-DN $ssl_client_s_dn;
        proxy_set_header X-Client-Serial $ssl_client_serial;
        proxy_set_header X-Client-Verify $ssl_client_verify;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - High Performance TLS Tuning and Session Resumption](./04-High-Performance-TLS-Tuning-and-Session-Resumption.md) | [Index](../../../README.md) | [06 - SSL Labs A Plus Configuration and Cipher Suites →](./06-SSL-Labs-A-Plus-Configuration-and-Cipher-Suites.md) |
