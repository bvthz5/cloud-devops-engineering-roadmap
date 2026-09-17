# 07 — Step-by-Step Apache SSL Setup with Certbot

## 1. Preparing Apache VirtualHost (`/etc/apache2/sites-available/example.com.conf`)

```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

```bash
# Enable VirtualHost and mod_ssl
sudo a2ensite example.com.conf
sudo a2enmod ssl rewrite
sudo apache2ctl configtest
sudo systemctl reload apache2
```

## 2. Executing Certbot Apache Plugin

```bash
# Automated Apache configuration modification & certificate issuance
sudo certbot --apache -d example.com -d www.example.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Nginx SSL Setup](./06-Step-by-Step-Nginx-SSL-Setup-with-Certbot.md) | [README](./README.md) | [08 - SSL File Structure & Permissions](./08-SSL-Certificate-Files-Structure-and-Permissions.md) |
