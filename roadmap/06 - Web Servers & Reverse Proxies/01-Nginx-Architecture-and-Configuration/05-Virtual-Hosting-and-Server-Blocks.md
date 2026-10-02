# 05 - Virtual Hosting and Server Blocks

## 1. Virtual Hosting Fundamentals

Virtual hosting allows a single physical Nginx server (or container) to host dozens of distinct domains (e.g., `api.example.com`, `admin.example.com`, `shop.org`) on a single IP address and port (80/443).

Nginx resolves which `server` block processes a request using:
1. The **IP address and Port** specified in the `listen` directive.
2. The **`Host` HTTP header** matched against the `server_name` directive.
3. TLS Server Name Indication (**SNI**) during the TLS handshake.

---

## 2. Default Catch-All Server Block

To prevent unexpected traffic, security scanning probes, or unmapped DNS records from accessing internal virtual hosts, configure a strict default catch-all block that returns HTTP 444 (Nginx non-standard code that closes connection without sending headers):

```nginx
# Default Catch-All Server (IPv4 & IPv6)
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;
    server_name _;

    # Dummy self-signed certificate for the catch-all SSL handshake
    ssl_certificate /etc/nginx/ssl/dummy.crt;
    ssl_certificate_key /etc/nginx/ssl/dummy.key;

    # Drop connection immediately without response
    return 444;
}
```

---

## 3. Name-Based Virtual Hosts with Wildcards and Regex

```nginx
# Virtual Host 1: Standard FQDN
server {
    listen 443 ssl http2;
    server_name app.company.com;
    root /var/www/app;
}

# Virtual Host 2: Wildcard Subdomains
server {
    listen 443 ssl http2;
    server_name *.tenant.company.com;
    root /var/www/tenants;
}

# Virtual Host 3: Regex with Named Capture Groups
server {
    listen 443 ssl http2;
    server_name ~^(?<customer>[a-z0-9]+)\.cloud\.company\.com$;

    location / {
        proxy_pass http://customer_upstream;
        proxy_set_header X-Customer-ID $customer;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - URL Rewriting Redirection and Returns](./04-URL-Rewriting-Redirection-and-Returns.md) | [Index](../../../README.md) | [06 - Nginx Performance Tuning and Kernel Directives →](./06-Nginx-Performance-Tuning-and-Kernel-Directives.md) |
