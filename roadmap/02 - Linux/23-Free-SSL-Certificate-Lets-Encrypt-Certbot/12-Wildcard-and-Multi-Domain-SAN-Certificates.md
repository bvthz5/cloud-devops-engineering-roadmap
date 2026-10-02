# 12 — Wildcard and Multi-Domain SAN Certificates

## 1. What are Wildcard & SAN Certificates?
- **Wildcard Certificate (`*.example.com`)**: Secures root domain `example.com` and **all first-level subdomains** (`api.example.com`, `app.example.com`, `dev.example.com`) with a single certificate!
- **Subject Alternative Name (SAN)**: Secures multiple completely distinct domain names (`example.com`, `example.org`, `mycompany.io`) under one single certificate file.

## 2. Obtaining Wildcard Certificates via Cloudflare DNS Plugin

```bash
# 1. Install Certbot Cloudflare DNS Plugin
sudo snap install certbot-dns-cloudflare

# 2. Create Cloudflare API Token credentials file (/etc/letsencrypt/cloudflare.ini)
sudo mkdir -p /etc/letsencrypt
sudo cat <<'EOF' > /etc/letsencrypt/cloudflare.ini
dns_cloudflare_api_token = YOUR_CLOUDFLARE_API_TOKEN_HERE
EOF
sudo chmod 600 /etc/letsencrypt/cloudflare.ini

# 3. Request Wildcard SSL Certificate using DNS-01 Challenge
sudo certbot certonly \
  --dns-cloudflare \
  --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini \
  -d example.com \
  -d "*.example.com" \
  --agree-tos \
  --no-eff-email \
  -m admin@example.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Validation Challenges](./11-Validation-Challenges-HTTP-01-vs-DNS-01.md) | [README](./README.md) | [13 - Cloudflare Proxy & SSL Modes](./13-Cloudflare-Proxy-Integration-and-SSL-Modes.md) |
