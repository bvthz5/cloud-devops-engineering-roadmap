# 13 — Cloudflare Proxy Integration and SSL Modes

## 1. Cloudflare SSL Architecture Modes

```
Client Browser  ======(HTTPS)======>  Cloudflare CDN Edge  ======(HTTP or HTTPS)======>  Origin Server
```

| SSL Mode | Edge-to-Browser | Edge-to-Origin | Security Risk / Recommended Usage |
|---|---|---|---|
| **Flexible** | HTTPS | **Unencrypted HTTP** | **INSECURE**: Traffic between Cloudflare and Origin is unencrypted. Susceptible to MITM. |
| **Full** | HTTPS | HTTPS (Self-Signed allowed) | Encrypted, but does not validate Origin certificate authenticity. |
| **Full (Strict)** | HTTPS | **HTTPS (Valid Cert Required)** | **RECOMMENDED**: Full end-to-end encryption with Let's Encrypt certificate on Origin! |

## 2. Fixing Infinite Redirect Loops (`ERR_TOO_MANY_REDIRECTS`)
- **Symptom**: Web page loops infinitely between HTTP and HTTPS when behind Cloudflare.
- **Root Cause**: Cloudflare SSL mode set to *Flexible*, but Origin server forces 301 HTTPS redirect!
- **Fix**: Switch Cloudflare SSL mode to **Full (Strict)** in Cloudflare SSL/TLS Settings tab.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Wildcard & SAN Certs](./12-Wildcard-and-Multi-Domain-SAN-Certificates.md) | [README](./README.md) | [14 - SSL/TLS Troubleshooting Guide](./14-SSL-TLS-Troubleshooting-Guide.md) |
