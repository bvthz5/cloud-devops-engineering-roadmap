# 20 — Scenario-Based Troubleshooting Drills

## Drill 01: The Unexpected Renewal Failure Notification
- **Scenario**: Certbot automated renewal fails via cron/timer.
- **Root Cause**: Firewall port 80 was closed after initial installation.
- **Fix**: Re-open port 80 (`sudo ufw allow 80/tcp`) or switch to DNS-01 challenge.

## Drill 02: `ERR_TOO_MANY_REDIRECTS` behind Cloudflare
- **Scenario**: Users encounter infinite redirect loops.
- **Root Cause**: Cloudflare SSL mode is *Flexible*, but origin forces HTTPS.
- **Fix**: Change Cloudflare SSL setting to **Full (Strict)**.

## Drill 03: Browser Security Warning After Renewal
- **Scenario**: Certbot successfully renewed certificate, but browser still sees old expired certificate.
- **Root Cause**: Nginx was not reloaded after renewal; active worker processes still serve old certificate in memory.
- **Fix**: Run `systemctl reload nginx` and add `--deploy-hook "systemctl reload nginx"` to renewal configuration.

## Drill 04: Let's Encrypt Staging Certificate in Production
- **Scenario**: Browser displays `NET::ERR_CERT_AUTHORITY_INVALID` ("Fake LE Intermediate X1").
- **Root Cause**: Certbot was executed with `--staging` or `--test-cert` flag during initial request.
- **Fix**: Re-issue without staging flag: `sudo certbot --nginx --force-renewal`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [19 - Hands-On Labs 04-10](./19-Hands-On-Labs-04-to-10-Renewal-Hardening-Cloudflare.md) | [README](./README.md) | [21 - Senior DevOps Interview Q&A](./21-DevOps-SSL-Interview-Questions-and-Answers.md) |
