# 25 — Production SSL Deployment Checklist

## 25-Point Production SSL/TLS Readiness Checklist

### DNS & Network Prerequisites
- [ ] 1. DNS A/AAAA records point to correct public server IP.
- [ ] 2. Firewall ports 80 (HTTP) and 443 (HTTPS) open in UFW/iptables and Cloud Security Groups.
- [ ] 3. Server hostname configured and resolvable.

### Certificate Issuance & Certbot Setup
- [ ] 4. Certbot installed via recommended Snapd package manager.
- [ ] 5. Admin contact email provided for security alert notices.
- [ ] 6. Certificate successfully issued via HTTP-01 or DNS-01 challenge.
- [ ] 7. Certificate files stored in `/etc/letsencrypt/live/example.com/`.

### Web Server Configuration (Nginx / Apache)
- [ ] 8. Nginx `ssl_certificate` directive configured to `fullchain.pem`.
- [ ] 9. Nginx `ssl_certificate_key` directive configured to `privkey.pem` (`0600` permissions).
- [ ] 10. Automatic HTTP (Port 80) to HTTPS (Port 443) 301 permanent redirect configured.
- [ ] 11. Weak legacy SSL/TLS protocols disabled (`TLSv1.2` and `TLSv1.3` enabled only).
- [ ] 12. Weak 64-bit/export ciphers disabled; modern AEAD ciphers configured.
- [ ] 13. Custom Diffie-Hellman parameters (2048-bit minimum) generated.

### Automation & Renewal
- [ ] 14. Systemd `certbot.timer` active and running (`systemctl status certbot.timer`).
- [ ] 15. Tested renewal simulation (`certbot renew --dry-run`) passed without errors.
- [ ] 16. Web server post-renewal deploy hook configured (`--deploy-hook "systemctl reload nginx"`).

### Performance & Security Hardening
- [ ] 17. HTTP Strict Transport Security (HSTS) header enabled (`max-age=31536000`).
- [ ] 18. OCSP Stapling enabled for fast certificate status validation.
- [ ] 19. HTTP/2 or HTTP/3 multiplexing enabled on port 443.
- [ ] 20. All web page assets (images, CSS, JS) loaded over relative/HTTPS paths (zero mixed content warnings).
- [ ] 21. Cloudflare SSL setting configured to **Full (Strict)** (if using Cloudflare CDN).

### Verification & Monitoring
- [ ] 22. Verified HTTPS response headers with `curl -Iv https://example.com`.
- [ ] 23. Verified raw certificate details via `openssl s_client -connect example.com:443`.
- [ ] 24. Achieved Grade A+ on Qualys SSL Labs Test (https://www.ssllabs.com/ssltest/).
- [ ] 25. External certificate expiry monitoring alert configured (PagerDuty/Datadog/UptimeRobot).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [24 - Certbot Command Cheat Sheet](./24-Complete-Certbot-Command-Cheat-Sheet.md) | [README](./README.md) | [README (Index)](./README.md) |
