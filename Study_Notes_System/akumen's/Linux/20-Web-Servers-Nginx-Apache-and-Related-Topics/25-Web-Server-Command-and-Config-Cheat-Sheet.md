# 25. Web Server Command & Config Cheat Sheet

```bash
# Nginx Management
sudo nginx -t                          # Test Nginx configuration syntax
sudo nginx -s reload                   # Zero-downtime config reload
sudo systemctl status nginx            # Check Nginx service status

# Apache Management
sudo apache2ctl configtest             # Test Apache configuration syntax
sudo systemctl reload apache2          # Reload Apache service
sudo a2ensite mysite.conf              # Enable virtual host site
sudo a2dissite mysite.conf             # Disable virtual host site
sudo a2enmod proxy_http                # Enable Apache module

# SSL & Certbot
sudo certbot --nginx -d example.com    # Obtain Let's Encrypt SSL for Nginx
sudo certbot --apache -d example.com   # Obtain Let's Encrypt SSL for Apache
sudo certbot renew --dry-run           # Test SSL certificate auto-renewal

# Log Inspection & Debugging
sudo tail -f /var/log/nginx/error.log  # Real-time error log monitoring
sudo netstat -tulpn | grep -E ':80|:443' # Check process listening on ports 80/443
curl -I -v https://example.com         # Inspect HTTP response headers & TLS handshake
```
