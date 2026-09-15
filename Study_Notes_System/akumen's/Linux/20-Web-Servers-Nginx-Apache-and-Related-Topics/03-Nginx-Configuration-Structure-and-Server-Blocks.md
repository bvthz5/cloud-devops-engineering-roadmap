# 3. Nginx Configuration Structure & Server Blocks

## Configuration Context Hierarchy

Nginx directives are arranged in nested contexts:

```nginx
# 1. Main / Global Context
user www-data;
worker_processes auto;
pid /run/nginx.pid;

# 2. Events Context
events {
    worker_connections 1024; # Max concurrent connections per worker
    use epoll;
}

# 3. HTTP Context
http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65;

    # 4. Server Context (Virtual Host)
    server {
        listen 80;
        server_name example.com www.example.com;
        root /var/www/html;

        # 5. Location Context
        location / {
            try_files $uri $uri/ =404;
        }

        location /api/ {
            proxy_pass http://127.0.0.1:3000;
        }
    }
}
```

## Location Block Modifier Precedence

Nginx selects location blocks based on strict prefix and regex matching precedence rules:

| Prefix | Modifier Type | Matching Behavior | Priority |
| --- | --- | --- | --- |
| `=` | Exact Match | Exact URI string match. Stops search immediately if matched. | 1 (Highest) |
| `^~` | Preferential Prefix | If longest matching prefix has `^~`, stop regex searching. | 2 |
| `~` | Case-Sensitive Regex | Regular expression match (case sensitive). | 3 |
| `~*` | Case-Insensitive Regex | Regular expression match (case insensitive). | 3 |
| *(none)* | Standard Prefix | Standard prefix matching. Selected if no regex matches. | 4 (Lowest) |

### Matching Example:
```nginx
location = /favicon.ico { ... }    # Matches ONLY /favicon.ico
location ^~ /images/ { ... }       # Matches /images/logo.png, skips regex
location ~* \.(png|jpg|css)$ { ... } # Matches static assets case-insensitively
location / { ... }                  # Fallback default catch-all
```
