# 03 - Automated Certificates with ACME and Certbot

## 1. The ACME Protocol (RFC 8555)

The **Automated Certificate Management Environment (ACME)** protocol completely automates certificate generation, validation, and renewal without human intervention.

```text
[ Certbot / Traefik / cert-manager ] ──(1. Request Cert)──► [ Let's Encrypt ACME Server ]
                                                                       │
[ Web Server ] ◄──(2. Challenge Token: GET /.well-known/acme-challenge/...)┤
      │                                                                │
      └───(3. Proves Ownership)────────────────────────────────────────┘
```

---

## 2. Challenge Types: HTTP-01 vs DNS-01

| Attribute | HTTP-01 Challenge | DNS-01 Challenge |
|---|---|---|
| **Mechanism** | Places a challenge file under `/.well-known/acme-challenge/` on port 80. | Creates a `_acme-challenge.domain.com` `TXT` DNS record via DNS API. |
| **Wildcard Support**| **No** (Cannot issue `*.example.com`). | **Yes** (Mandatory for wildcard certificates). |
| **Firewall Requirement**| Port 80 must be reachable from public internet. | Zero open inbound ports required! Works for private internal VPN servers. |

### 2.1 Generating Certificates with Certbot (Nginx Plugin)
```bash
# Install Certbot and Nginx plugin
sudo apt-get install -y certbot python3-certbot-nginx

# Request certificate and configure Nginx automatically
sudo certbot --nginx -d example.com -d www.example.com --agree-tos --email admin@example.com

# Non-interactive renewal dry-run
sudo certbot renew --dry-run
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Certificate Authorities Chains and Trust Stores](./02-Certificate-Authorities-Chains-and-Trust-Stores.md) | [Index](../../../README.md) | [04 - High Performance TLS Tuning and Session Resumption →](./04-High-Performance-TLS-Tuning-and-Session-Resumption.md) |
