# 05 — Certbot Installation Methods: Snap vs Apt

## 1. Recommended Method: Snapd (Official EFF Standard)
The Electronic Frontier Foundation (EFF) officially recommends installing Certbot via `snapd` to ensure you always run the latest version with up-to-date ACME protocol compatibility.

```bash
# 1. Update snapd core
sudo snap install core; sudo snap refresh core

# 2. Remove any legacy OS certbot packages
sudo apt-get remove certbot -y 2>/dev/null || true

# 3. Install Certbot via Snap
sudo snap install --classic certbot

# 4. Create symlink to /usr/bin/certbot
sudo ln -sf /snap/bin/certbot /usr/bin/certbot

# 5. Verify installation version
certbot --version
```

## 2. Alternative Method: APT Package Manager (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install -y certbot python3-certbot-nginx python3-certbot-apache
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Firewall Setup](./04-Firewall-Configuration-UFW-and-Cloud-Security-Groups.md) | [README](./README.md) | [06 - Nginx SSL Setup with Certbot](./06-Step-by-Step-Nginx-SSL-Setup-with-Certbot.md) |
