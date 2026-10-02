# 06 — Step-by-Step Nginx SSL Setup with Certbot

## 1. Preparing Nginx Server Block (`/etc/nginx/sites-available/example.com`)

Ensure your Nginx configuration contains a valid `server_name` directive:

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

```bash
# Enable site and test configuration syntax
sudo ln -sf /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

## 2. Running Certbot Nginx Plugin

```bash
# Interactive command: automatically modifies Nginx config and configures 301 HTTPS redirect
sudo certbot --nginx -d example.com -d www.example.com
```

### Prompt Interaction Guide
1. **Email Prompt**: Enter security alert contact email address.
2. **Terms of Service**: Agree (`Y`).
3. **Redirect Prompt**: Choose `2: Redirect - Make all requests redirect to secure HTTPS access`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Certbot Installation](./05-Certbot-Installation-Methods-Snap-vs-Apt.md) | [README](./README.md) | [07 - Apache SSL Setup with Certbot](./07-Step-by-Step-Apache-SSL-Setup-with-Certbot.md) |
