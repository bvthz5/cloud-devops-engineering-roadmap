# 6. TLS / HTTPS, SSL Certificates & Certbot

## SSL/TLS Handshake Overview

```text
[ Client ] ────────────── 1. Client Hello (Supported TLS versions, ciphers) ─────────────► [ Nginx ]
[ Client ] ◄───────────── 2. Server Hello + Certificate + Public Key ──────────────────── [ Nginx ]
[ Client ] ────────────── 3. Verify Cert via CA Root + Send Encrypted Pre-Master ────────► [ Nginx ]
[ Client ] ◄────────────── 4. Switch to Symmetric Session Key (AES-GCM) ────────────────► [ Nginx ]
```

## Hardened Nginx SSL Server Block Configuration

```nginx
server {
    listen 443 ssl http2;
    server_name example.com;

    # SSL Certificate Paths
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # TLS Protocols & Ciphers
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    # SSL Session Optimization
    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:10m; # ~40,000 sessions
    ssl_session_tickets off;

    # OCSP Stapling (Fast revocation checking)
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 1.1.1.1 valid=300s;

    # HTTP Strict Transport Security (HSTS)
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;

    location / {
        root /var/www/html;
    }
}

# HTTP to HTTPS Redirect Block
server {
    listen 80;
    server_name example.com www.example.com;
    return 301 https://$host$request_uri;
}
```

## Let's Encrypt Automation with Certbot

```bash
# Install Certbot Nginx plugin
sudo apt update && sudo apt install -y certbot python3-certbot-nginx

# Obtain and automatically install SSL certificate
sudo certbot --nginx -d example.com -d www.example.com

# Test automatic renewal timer / cron job
sudo certbot renew --dry-run
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Nginx Load Balancing Algorithms and Health Checks](./05-Nginx-Load-Balancing-Algorithms-and-Health-Checks.md) | [README](./README.md) | [07 - Caching Compression Gzip Brotli and HTTP Headers](./07-Caching-Compression-Gzip-Brotli-and-HTTP-Headers.md) |
