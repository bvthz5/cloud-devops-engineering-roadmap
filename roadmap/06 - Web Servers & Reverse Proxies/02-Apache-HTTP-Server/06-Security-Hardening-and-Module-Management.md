# 06 - Security Hardening and Module Management

## 1. Information Disclosure Suppression

By default, Apache advertises its exact version, operating system distribution, and installed modules in the HTTP `Server` response header and error pages.

```apache
# /etc/apache2/conf-available/security.conf

# Do not leak exact Apache version or OS details
ServerTokens Prod

# Remove server version signature from generated error pages
ServerSignature Off

# Prevent TRACE method exploitation (Cross-Site Tracing)
TraceEnable Off
```

---

## 2. Hardening Headers with `mod_headers`

```apache
<IfModule mod_headers.c>
    # Prevent MIME sniffing
    Header always set X-Content-Type-Options "nosniff"

    # Clickjacking protection
    Header always set X-Frame-Options "SAMEORIGIN"

    # Enforce HTTPS via HSTS (1 year)
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"

    # Referrer policy
    Header always set Referrer-Policy "strict-origin-when-cross-origin"
</IfModule>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Reverse Proxying](./05-Reverse-Proxying-with-mod-proxy.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
