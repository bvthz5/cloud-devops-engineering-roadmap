# 18 — Hands-On Lab 03: Wildcard Certificate via DNS-01 Challenge

## Lab Objective
Obtain a Wildcard SSL Certificate (`*.example.com`) using Certbot and the Cloudflare DNS API plugin via DNS-01 verification challenge.

## Step-by-Step Lab Instructions

### Step 1: Install Cloudflare DNS Plugin for Certbot
```bash
sudo snap install certbot-dns-cloudflare
```

### Step 2: Configure Cloudflare Credentials
```bash
sudo mkdir -p /etc/letsencrypt
sudo cat <<'EOF' > /etc/letsencrypt/cloudflare.ini
dns_cloudflare_api_token = 1234567890abcdef1234567890abcdef
EOF

sudo chmod 600 /etc/letsencrypt/cloudflare.ini
```

### Step 3: Issue Wildcard Certificate
```bash
sudo certbot certonly \
  --dns-cloudflare \
  --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini \
  -d example.com \
  -d "*.example.com" \
  --agree-tos \
  -m admin@example.com
```

### Step 4: Verify Issued Wildcard Files
```bash
sudo ls -l /etc/letsencrypt/live/example.com/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [17 - Lab 02: Apache Certbot](./17-Hands-On-Lab-02-Apache-Certbot-Setup.md) | [README](./README.md) | [19 - Hands-On Labs 04 to 10](./19-Hands-On-Labs-04-to-10-Renewal-Hardening-Cloudflare.md) |
