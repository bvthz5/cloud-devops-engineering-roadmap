# 04 - High-Performance TLS Tuning and Session Resumption

## 1. OCSP Stapling (RFC 6066)

Under standard Online Certificate Status Protocol (OCSP), when a browser connects to your website, it queries the Certificate Authority's OCSP server to verify the certificate has not been revoked, introducing a latency penalty of 100-300ms.

With **OCSP Stapling**:
The Nginx server proactively queries the CA's OCSP server in the background, caches the cryptographically signed timestamped revocation status, and "staples" it directly to the TLS handshake payload.

```nginx
# High-Performance OCSP Stapling in Nginx
ssl_stapling on;
ssl_stapling_verify on;

# Specify trusted resolver for background OCSP queries
resolver 1.1.1.1 8.8.8.8 valid=300s;
resolver_timeout 5s;

# Specify root CA certificate to verify stapled OCSP response
ssl_trusted_certificate /etc/letsencrypt/live/example.com/chain.pem;
```

---

## 2. TLS Session Resumption

```nginx
# Cache TLS sessions in shared memory across worker processes (10MB holds ~40,000 sessions)
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 1d;

# Disable session tickets for forward secrecy, or ensure ticket keys are rotated regularly
ssl_session_tickets off;
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Automated Certificates with ACME & Certbot](./03-Automated-Certificates-with-ACME-and-Certbot.md) | [README](./README.md) | [05 - Mutual TLS (mTLS) & Zero-Trust](./05-Mutual-TLS-mTLS-Architecture-and-Zero-Trust.md) |
