# 24 — Complete Certbot Command Cheat Sheet

## Certbot CLI Command Reference

```bash
# 1. Automatic Nginx SSL Setup & Config
sudo certbot --nginx -d example.com -d www.example.com

# 2. Automatic Apache SSL Setup & Config
sudo certbot --apache -d example.com -d www.example.com

# 3. Obtain Certificate Only (No web server config edit)
sudo certbot certonly --webroot -w /var/www/html -d example.com

# 4. Standalone Mode (Temporary built-in web server on Port 80)
sudo certbot certonly --standalone -d example.com

# 5. Wildcard Certificate via Cloudflare DNS Plugin
sudo certbot certonly --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini -d example.com -d "*.example.com"

# 6. List Active Installed Certificates & Expiry Dates
sudo certbot certificates

# 7. Test Automated Renewal Workflow
sudo certbot renew --dry-run

# 8. Force Immediate Renewal
sudo certbot renew --force-renewal

# 9. Delete Installed Certificate
sudo certbot delete --cert-name example.com

# 10. Revoke Compromised Certificate
sudo certbot revoke --cert-path /etc/letsencrypt/live/example.com/cert.pem
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [23 - Quick Revision Notes](./23-Quick-Revision-Notes.md) | [README](./README.md) | [25 - Production SSL Deployment Checklist](./25-Production-SSL-Deployment-Checklist.md) |
