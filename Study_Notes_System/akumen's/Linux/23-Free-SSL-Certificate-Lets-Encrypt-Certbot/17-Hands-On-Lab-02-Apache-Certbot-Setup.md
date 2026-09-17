# 17 — Hands-On Lab 02: Apache Certbot Setup

## Lab Objective
Configure Apache HTTP Server on Ubuntu, enable `mod_ssl` and `mod_rewrite`, execute Certbot Apache plugin, and verify SSL VirtualHost configuration.

## Step-by-Step Lab Instructions

### Step 1: Install Apache Web Server
```bash
sudo apt update && sudo apt install -y apache2
sudo systemctl enable --now apache2
```

### Step 2: Configure VirtualHost (`/etc/apache2/sites-available/demo2.example.com.conf`)
```apache
<VirtualHost *:80>
    ServerName demo2.example.com
    DocumentRoot /var/www/html
</VirtualHost>
```

```bash
sudo a2ensite demo2.example.com.conf
sudo a2enmod ssl rewrite
sudo apache2ctl configtest
sudo systemctl reload apache2
```

### Step 3: Run Certbot Apache Plugin
```bash
sudo certbot --apache -d demo2.example.com --agree-tos --redirect -m admin@example.com
```

### Step 4: Verify Apache SSL VirtualHost Creation
```bash
cat /etc/apache2/sites-available/demo2.example.com-le-ssl.conf
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - Lab 01: Nginx Certbot](./16-Hands-On-Lab-01-Nginx-Certbot-HTTP01.md) | [README](./README.md) | [18 - Lab 03: Wildcard DNS-01 Challenge](./18-Hands-On-Lab-03-Wildcard-DNS01-Challenge.md) |
