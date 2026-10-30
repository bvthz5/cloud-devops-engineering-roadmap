# 10 - Hands-On Practice Labs

## Lab 1: Hardened Apache Reverse Proxy with Security Headers

### Objective
Configure Apache with `mod_proxy` forwarding traffic to a backend application on port 5000 while enforcing HTTPS and enterprise security headers.

### Implementation
```apache
<VirtualHost *:80>
    ServerName service.example.com
    RewriteEngine On
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</VirtualHost>

<VirtualHost *:443>
    ServerName service.example.com

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/service.crt
    SSLCertificateKeyFile /etc/ssl/private/service.key

    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/

    Header always set X-Frame-Options "DENY"
    Header always set X-Content-Type-Options "nosniff"
</VirtualHost>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
