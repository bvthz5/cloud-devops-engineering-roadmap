# 19 — Hands-On Labs 04 to 10: Renewal, Hardening, and Cloudflare

## Lab 04: Testing Renewal Simulation (`--dry-run`)
```bash
sudo certbot renew --dry-run
```

## Lab 05: Adding Deploy Hooks for Service Reloads
```bash
sudo certbot renew --deploy-hook "systemctl reload nginx"
```

## Lab 06: Generating Custom Diffie-Hellman Parameters
```bash
sudo openssl dhparam -out /etc/nginx/dhparam.pem 2048
```

## Lab 07: Enabling HSTS Header in Nginx
Add to server block:
```nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
```

## Lab 08: Configuring OCSP Stapling
Add to Nginx config:
```nginx
ssl_stapling on;
ssl_stapling_verify on;
ssl_trusted_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
```

## Lab 09: Configuring Cloudflare Full (Strict) SSL Mode
1. Set Cloudflare SSL/TLS mode to **Full (Strict)**.
2. Install origin certificate on Nginx.

## Lab 10: Revoking an Compromised SSL Certificate
```bash
sudo certbot revoke --cert-path /etc/letsencrypt/live/example.com/cert.pem
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [18 - Lab 03: Wildcard DNS-01](./18-Hands-On-Lab-03-Wildcard-DNS01-Challenge.md) | [README](./README.md) | [20 - Scenario Troubleshooting Drills](./20-Scenario-Based-Troubleshooting-Drills.md) |
