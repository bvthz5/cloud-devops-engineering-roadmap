# 04 — Firewall Configuration: UFW and Cloud Security Groups

## 1. Local Firewall Setup (UFW - Ubuntu/Debian)

```bash
# Check UFW status
sudo ufw status

# Allow Nginx Full (Port 80 and 443)
sudo ufw allow 'Nginx Full'

# Or allow Apache Full (Port 80 and 443)
sudo ufw allow 'Apache Full'

# Or manually allow explicit ports:
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Reload firewall
sudo ufw reload
```

## 2. Cloud Security Groups (AWS EC2 / GCP / Azure)

Ensure inbound ingress rules are allowed on your Cloud Provider dashboard:

| Type | Protocol | Port Range | Source | Purpose |
|---|---|---|---|---|
| HTTP | TCP | 80 | `0.0.0.0/0` | ACME HTTP-01 challenge & HTTP traffic |
| HTTPS | TCP | 443 | `0.0.0.0/0` | Encrypted web application traffic |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Prerequisites & DNS](./03-Prerequisites-DNS-Records-and-Network-Ports.md) | [README](./README.md) | [05 - Certbot Installation Methods](./05-Certbot-Installation-Methods-Snap-vs-Apt.md) |
