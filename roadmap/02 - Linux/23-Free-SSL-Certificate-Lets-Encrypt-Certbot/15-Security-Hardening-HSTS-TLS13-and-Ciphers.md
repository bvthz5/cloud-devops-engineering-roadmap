# 15 — Security Hardening: HSTS, TLS 1.3, and Ciphers

## 1. Restricting Insecure Protocols
Modern TLS configurations disable legacy TLS 1.0 and TLS 1.1 (vulnerable to BEAST, POODLE attacks).

### Nginx Hardened Protocol Directive
```nginx
ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers off;
```

## 2. HTTP Strict Transport Security (HSTS)
HSTS informs browsers to **ONLY connect to the site using HTTPS**, preventing SSL Stripping MITM attacks.

```nginx
# Enable HSTS for 1 year including subdomains
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
```

## 3. OCSP Stapling (Online Certificate Status Protocol)
Improves SSL handshake speed by serving cached cryptographic proof of certificate validity directly from web server to client.

```nginx
ssl_stapling on;
ssl_stapling_verify on;
ssl_trusted_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
resolver 1.1.1.1 8.8.8.8 valid=300s;
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - SSL Troubleshooting Guide](./14-SSL-TLS-Troubleshooting-Guide.md) | [README](./README.md) | [16 - Lab 01: Nginx Certbot Setup](./16-Hands-On-Lab-01-Nginx-Certbot-HTTP01.md) |
