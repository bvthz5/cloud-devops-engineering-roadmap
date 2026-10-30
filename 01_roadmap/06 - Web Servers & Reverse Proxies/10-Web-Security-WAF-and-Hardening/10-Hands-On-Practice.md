# 10 - Hands-On Practice Labs

## Lab 1: Hardened Reverse Proxy with Security Headers and Slowloris Defense

### Objective
Configure Nginx with a full suite of enterprise security headers, aggressive timeout guards, and server token suppression.

### Implementation
```nginx
server {
    listen 443 ssl http2;
    server_name portal.example.com;

    ssl_certificate /etc/ssl/certs/portal.crt;
    ssl_certificate_key /etc/ssl/private/portal.key;

    # 1. Information Disclosure Suppression
    server_tokens off;

    # 2. Strict Timeouts (Slowloris Protection)
    client_body_timeout 10s;
    client_header_timeout 10s;
    keepalive_timeout 20s;
    send_timeout 10s;

    # 3. Security Headers
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "DENY" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Content-Security-Policy "default-src 'self';" always;

    location / {
        proxy_pass http://internal_portal;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_set_header Host $host;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
