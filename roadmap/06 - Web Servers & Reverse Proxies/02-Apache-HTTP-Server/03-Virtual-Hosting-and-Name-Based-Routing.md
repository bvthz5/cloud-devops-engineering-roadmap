# 03 - Virtual Hosting and Name-Based Routing

## 1. Name-Based Virtual Hosts

Apache handles multiple domains on a single IP address using `<VirtualHost>` containers matched against the incoming `Host` header.

```apache
# /etc/apache2/sites-available/app.example.com.conf
<VirtualHost *:80>
    ServerName app.example.com
    ServerAlias www.app.example.com
    ServerAdmin admin@example.com

    DocumentRoot /var/www/app

    ErrorLog ${APACHE_LOG_DIR}/app_error.log
    CustomLog ${APACHE_LOG_DIR}/app_access.log combined

    <Directory /var/www/app>
        Options -Indexes +FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

---

## 2. Managing Sites on Debian/Ubuntu

Debian and Ubuntu provide helper CLI utilities for managing virtual hosts and Apache modules:

```bash
# Enable or disable a site
sudo a2ensite app.example.com.conf
sudo a2dissite 000-default.conf

# Enable or disable Apache modules
sudo a2enmod rewrite ssl proxy proxy_http headers
sudo a2dismod autoindex

# Test configuration before reloading
sudo apache2ctl configtest
sudo systemctl reload apache2
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Configuration Hierarchy and Directive Scopes](./02-Configuration-Hierarchy-and-Directive-Scopes.md) | [Index](../../../README.md) | [04 - URL Rewriting with mod rewrite →](./04-URL-Rewriting-with-mod-rewrite.md) |
