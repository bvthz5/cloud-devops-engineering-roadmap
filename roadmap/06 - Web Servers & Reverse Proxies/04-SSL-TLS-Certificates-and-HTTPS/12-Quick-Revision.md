# 12 - Quick-Revision & Enterprise Cheat Sheet

```nginx
# Production SSL Baseline
ssl_protocols TLSv1.2 TLSv1.3;
ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256';
ssl_prefer_server_ciphers off;

# Session Caching & OCSP Stapling
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 1d;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 1.1.1.1 8.8.8.8 valid=300s;

# HSTS Header
add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
```

```bash
# Check expiry date of domain
openssl s_client -connect domain.com:443 -servername domain.com 2>/dev/null | openssl x509 -noout -enddate
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [05 - Caching & Rate Limiting](../05-Caching-and-Rate-Limiting/README.md) |
