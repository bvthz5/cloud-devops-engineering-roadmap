# 10. Apache Virtual Hosts & `mod_proxy` Configuration

## Name-Based Virtual Hosts

Apache uses `<VirtualHost>` blocks to host multiple websites on a single server IP address.

```apache
# /etc/apache2/sites-available/example.conf
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example

    <Directory /var/www/example>
        Options -Indexes +FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/example_error.log
    CustomLog ${APACHE_LOG_DIR}/example_access.log combined
</VirtualHost>
```

## Apache Reverse Proxy with `mod_proxy`

Enable proxy modules:
```bash
sudo a2enmod proxy proxy_http proxy_balancer lbmethod_byrequests rewrite
```

Reverse Proxy Virtual Host Configuration:
```apache
<VirtualHost *:80>
    ServerName app.example.com

    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/

    # Forwarding headers automatically added by mod_proxy
    # X-Forwarded-For, X-Forwarded-Host, X-Forwarded-Server

    ErrorLog ${APACHE_LOG_DIR}/proxy_error.log
    CustomLog ${APACHE_LOG_DIR}/proxy_access.log combined
</VirtualHost>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Apache HTTP Server Architecture and MPMs](./09-Apache-HTTP-Server-Architecture-and-MPMs.md) | [README](./README.md) | [11 - Nginx vs Apache Architecture Performance Comparison](./11-Nginx-vs-Apache-Architecture-Performance-Comparison.md) |
