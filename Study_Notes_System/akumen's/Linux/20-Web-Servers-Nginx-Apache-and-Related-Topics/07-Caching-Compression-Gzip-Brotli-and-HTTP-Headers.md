# 7. Caching, Compression & Security Headers

## Nginx Proxy Caching Architecture

Caching reduces backend server load by storing upstream HTTP responses on disk and serving repeated client requests directly from Nginx memory/disk.

```nginx
# Define Cache Zone in http context
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=MY_CACHE:10m max_size=1g inactive=60m use_temp_path=off;

server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_cache MY_CACHE;
        proxy_cache_valid 200 302 10m;
        proxy_cache_valid 404 1m;
        proxy_cache_use_stale error timeout updating http_500 http_502 http_503;
        
        # Add Header to inspect cache status (HIT, MISS, BYPASS)
        add_header X-Cache-Status $upstream_cache_status;

        proxy_pass http://127.0.0.1:3000;
    }
}
```

## Compression: Gzip & Brotli

```nginx
http {
    # Enable Gzip Compression
    gzip on;
    gzip_disable "msie6";
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml+rss text/javascript;
}
```

## Essential Web Security Headers

```nginx
# Protect against XSS, clickjacking, MIME sniffing
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "no-referrer-when-downgrade" always;
add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'" always;
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - TLS HTTPS SSL Certificates and Certbot](./06-TLS-HTTPS-SSL-Certificates-and-Certbot.md) | [README](./README.md) | [08 - WebSockets HTTP2 HTTP3 and gRPC Proxying](./08-WebSockets-HTTP2-HTTP3-and-gRPC-Proxying.md) |
