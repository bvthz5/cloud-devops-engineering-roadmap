# 14 — SSL/TLS Troubleshooting Guide

## 1. Top 5 SSL/TLS Failure Scenarios & Fixes

### Scenario 1: ACME Challenge 404 / Unauthorized
- **Symptom**: `Certbot failed to authenticate some domains (HTTP-01 challenge failed)`.
- **Root Cause**: Port 80 blocked by firewall, wrong Nginx `root` path, or domain DNS pointing to wrong server IP.
- **Fix**: Check `curl -I http://example.com/.well-known/acme-challenge/test.txt`. Ensure firewall allows port 80.

### Scenario 2: `SSL_ERROR_RX_RECORD_TOO_LONG`
- **Symptom**: Browser error when opening HTTPS URL.
- **Root Cause**: Server listening on port 443 with plain HTTP protocol instead of enabling SSL/TLS (`listen 443 ssl;` missing `ssl` keyword in Nginx!).
- **Fix**: Add `ssl` flag: `listen 443 ssl;`.

### Scenario 3: Mixed Content Warning
- **Symptom**: Browser shows padlock icon with yellow warning triangle ("Your connection to this site is not fully secure").
- **Root Cause**: HTML page loaded over HTTPS contains hardcoded `http://` images, scripts, or CSS resources.
- **Fix**: Update resource URLs to relative `/path` or `https://`. Add header: `Content-Security-Policy: upgrade-insecure-requests;`.

### Scenario 4: Let's Encrypt Rate Limit Exceeded
- **Symptom**: `Error creating new order :: too many certificates already issued for exact set of domains`.
- **Root Cause**: Let's Encrypt limits exact domain issuance to 5 duplicate certs per week.
- **Fix**: Use `--staging` environment during testing before requesting production certificates!

### Scenario 5: Certificate Expired Alert
- **Symptom**: `NET::ERR_CERT_DATE_INVALID` in browser.
- **Root Cause**: `certbot.timer` failed or web server was not reloaded after renewal.
- **Fix**: `sudo certbot renew --force-renewal && sudo systemctl reload nginx`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - Cloudflare Proxy Modes](./13-Cloudflare-Proxy-Integration-and-SSL-Modes.md) | [README](./README.md) | [15 - Security Hardening & HSTS](./15-Security-Hardening-HSTS-TLS13-and-Ciphers.md) |
