# 09 — Verifying HTTPS and TLS Handshakes

## 1. Verifying HTTPS Response with `curl`

```bash
# Inspect HTTP headers and verify TLS certificate validation
curl -Iv https://example.com
```

### Expected Response Output Analysis
```text
* Connected to example.com (203.0.113.50) port 443
* ALPN: offers h2,http/1.1
* SSL connection using TLSv1.3 / TLS_AES_256_GCM_SHA384
* Server certificate:
*  subject: CN=example.com
*  start date: Sep 17 20:00:00 2026 GMT
*  expire date: Dec 16 20:00:00 2026 GMT
*  issuer: C=US; O=Let's Encrypt; CN=R3
*  SSL certificate verify ok.
< HTTP/2 200 
```

## 2. Inspecting Certificates with `openssl`

```bash
# Connect and print SSL certificate details
openssl s_client -servername example.com -connect example.com:443 </dev/null 2>/dev/null | openssl x509 -noout -dates -issuer -subject
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - SSL File Structure](./08-SSL-Certificate-Files-Structure-and-Permissions.md) | [README](./README.md) | [10 - Automated Renewal & Timers](./10-Automated-Renewal-Certbot-Timer-and-Cron.md) |
