# 16 — Hands-On Lab 01: Nginx Certbot Setup (HTTP-01 Challenge)

## Lab Objective
Set up Nginx on Ubuntu, configure an active domain name, install Certbot via Snap, issue a Let's Encrypt SSL certificate via HTTP-01 challenge, verify automatic 301 HTTPS redirection, and test TLS handshakes.

## Step-by-Step Lab Instructions

### Step 1: Install Nginx Web Server
```bash
sudo apt update && sudo apt install -y nginx
sudo systemctl enable --now nginx
```

### Step 2: Configure Nginx Server Block
```bash
sudo cat <<'EOF' > /etc/nginx/sites-available/demo.example.com
server {
    listen 80;
    server_name demo.example.com;
    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

sudo ln -sf /etc/nginx/sites-available/demo.example.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### Step 3: Install Certbot via Snapd
```bash
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -sf /snap/bin/certbot /usr/bin/certbot
```

### Step 4: Run Certbot Nginx Automation
```bash
sudo certbot --nginx -d demo.example.com --agree-tos --redirect -m admin@example.com
```

### Step 5: Verify HTTPS Response
```bash
curl -Iv https://demo.example.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [15 - Security Hardening](./15-Security-Hardening-HSTS-TLS13-and-Ciphers.md) | [README](./README.md) | [17 - Lab 02: Apache Certbot Setup](./17-Hands-On-Lab-02-Apache-Certbot-Setup.md) |
