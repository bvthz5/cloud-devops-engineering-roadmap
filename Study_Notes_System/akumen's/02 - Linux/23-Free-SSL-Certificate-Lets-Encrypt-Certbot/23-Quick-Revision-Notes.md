# 23 — Quick Revision Notes

## 1. Top 10 High-Yield SSL/TLS Cheat Notes

1. **Port 80 & 443**: Both must be open in firewall for HTTP-01 challenge and HTTPS traffic.
2. **Snapd Certbot**: EFF standard installation (`sudo snap install --classic certbot`).
3. **90-Day Validity**: Let's Encrypt certificates expire in 90 days; renewal recommended at 60 days.
4. **`fullchain.pem`**: Server cert + Intermediate CA chain (Nginx: `ssl_certificate`).
5. **`privkey.pem`**: Keep secret! Permissions `0600` owned by root (Nginx: `ssl_certificate_key`).
6. **Deploy Hook**: Reload web server after successful renewal (`--deploy-hook "systemctl reload nginx"`).
7. **DNS-01 Challenge**: Mandatory for wildcard certificates (`*.domain.com`).
8. **Cloudflare Mode**: Set to **Full (Strict)** when running Let's Encrypt on origin server.
9. **Dry Run Testing**: `sudo certbot renew --dry-run`.
10. **HSTS Header**: Forces HTTPS connection (`Strict-Transport-Security "max-age=31536000"`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [22 - Scenario MCQs](./22-MCQs-and-Diagnostic-Quizzes.md) | [README](./README.md) | [24 - Certbot Command Cheat Sheet](./24-Complete-Certbot-Command-Cheat-Sheet.md) |
