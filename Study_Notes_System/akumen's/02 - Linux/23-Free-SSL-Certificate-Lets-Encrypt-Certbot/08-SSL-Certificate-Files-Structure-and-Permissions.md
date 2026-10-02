# 08 — SSL Certificate Files Structure and Permissions

## 1. Directory Structure (`/etc/letsencrypt/`)

Certbot stores active certificates inside `/etc/letsencrypt/live/example.com/`:

```text
/etc/letsencrypt/
├── archive/
│   └── example.com/          (Stores actual raw certificate versions cert1.pem, cert2.pem...)
├── live/
│   └── example.com/          (Symlinks pointing to latest active files in archive/)
│       ├── cert.pem          -> Domain public certificate
│       ├── chain.pem         -> Let's Encrypt intermediate CA certificate
│       ├── fullchain.pem     -> Combined cert.pem + chain.pem (PREFERRED FOR WEB SERVERS)
│       └── privkey.pem       -> Private Key (CRITICAL SECRET!)
└── renewal/
    └── example.com.conf      -> Renewal configuration settings for Certbot
```

## 2. Certificate File Roles

| File Name | Purpose | Configuration Directive |
|---|---|---|
| `fullchain.pem` | Complete certificate chain (Server Cert + Intermediate CA Cert). | Nginx: `ssl_certificate` / Apache: `SSLCertificateFile` |
| `privkey.pem` | Private key associated with certificate. **Keep Secret!** | Nginx: `ssl_certificate_key` / Apache: `SSLCertificateKeyFile` |

## 3. Recommended Permissions
- `/etc/letsencrypt/live/`: `0755` permissions owned by `root:root`.
- `privkey.pem`: `0600` permissions (readable ONLY by `root`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Apache SSL Setup](./07-Step-by-Step-Apache-SSL-Setup-with-Certbot.md) | [README](./README.md) | [09 - Verifying HTTPS & TLS Handshakes](./09-Verifying-HTTPS-and-TLS-Handshakes.md) |
