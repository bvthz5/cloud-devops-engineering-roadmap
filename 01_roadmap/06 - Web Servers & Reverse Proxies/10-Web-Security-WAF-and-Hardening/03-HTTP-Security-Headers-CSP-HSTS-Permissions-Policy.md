# 03 - HTTP Security Headers: CSP, HSTS & Permissions-Policy

## 1. Content Security Policy (CSP)

CSP is the most effective browser-enforced defense against Cross-Site Scripting (XSS) and data injection attacks. It restricts which origins can execute scripts, load stylesheets, or open WebSocket connections:

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted-cdn.com; img-src 'self' data:; object-src 'none'; frame-ancestors 'none';
```

---

## 2. Complete Enterprise Security Header Suite

Inject these headers globally across all Nginx server blocks:

```nginx
# 1. Enforce HTTPS for 2 years across all subdomains + preloading
add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;

# 2. Prevent MIME-type sniffing
add_header X-Content-Type-Options "nosniff" always;

# 3. Disallow iframe framing to stop clickjacking
add_header X-Frame-Options "DENY" always;

# 4. Restrict Referrer information in cross-origin requests
add_header Referrer-Policy "strict-origin-when-cross-origin" always;

# 5. Disable dangerous browser hardware features
add_header Permissions-Policy "geolocation=(), microphone=(), camera=()" always;

# 6. Hide server software identity
server_tokens off;
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - ModSecurity and Coraza with OWASP CRS](./02-ModSecurity-and-Coraza-with-OWASP-CRS.md) | [Index](../../../README.md) | [04 - Mitigating HTTP Request Smuggling and Splitting →](./04-Mitigating-HTTP-Request-Smuggling-and-Splitting.md) |
