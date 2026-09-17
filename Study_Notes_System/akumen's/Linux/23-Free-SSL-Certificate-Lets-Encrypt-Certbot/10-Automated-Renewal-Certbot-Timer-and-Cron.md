# 10 — Automated Renewal: Certbot Timer and Cron

## 1. Automatic Renewal Architecture
Let's Encrypt certificates are valid for **90 days**. Certbot automatically attempts renewal for any certificate expiring within **30 days**.

## 2. Systemd Timer Inspection (`certbot.timer`)
Installing Certbot via snapd or apt automatically enables a systemd timer `certbot.timer` running twice daily.

```bash
# Check status of certbot systemd timer
systemctl status certbot.timer

# List active systemd timers
systemctl list-timers | grep certbot
```

## 3. Testing Renewal Workflow (`--dry-run`)

```bash
# Simulate full renewal process without modifying live certificates
sudo certbot renew --dry-run
```

## 4. Configuring Post-Renewal Web Server Reload Hooks
When a certificate is successfully renewed, web servers (Nginx/Apache) must reload configuration to load the new certificate file from disk!

```bash
# Deploy renewal hook command
sudo certbot renew --deploy-hook "systemctl reload nginx"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Verifying HTTPS](./09-Verifying-HTTPS-and-TLS-Handshakes.md) | [README](./README.md) | [11 - Validation Challenges (HTTP-01 vs DNS-01)](./11-Validation-Challenges-HTTP-01-vs-DNS-01.md) |
