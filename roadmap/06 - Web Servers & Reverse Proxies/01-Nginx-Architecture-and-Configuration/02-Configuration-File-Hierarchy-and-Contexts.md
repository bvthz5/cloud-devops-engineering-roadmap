# 02 - Configuration File Hierarchy and Contexts

## 1. Nginx Context Hierarchy

Nginx configuration files are organized into a strict nested block structure known as **Contexts** (or scopes). Directives placed in an outer context are inherited by inner contexts unless explicitly overridden.

```text
[ main context ] ── Core system settings (worker_processes, pid, error_log)
       │
       ▼
 [ events context ] ── Connection processing settings (worker_connections, use epoll)
       │
       ▼
  [ http context ] ── Universal HTTP settings (mime.types, sendfile, keepalive_timeout)
       │
       ├──► [ upstream context ] ── Backend server pools for reverse proxying
       │
       └──► [ server context ] ── Virtual host definitions (listen 80/443, server_name)
                 │
                 └──► [ location context ] ── URI routing, proxy_pass, root directives
```

---

## 2. Directory Layout & Modular Configuration

Standard Linux distributions (Ubuntu, Debian, RHEL) structure Nginx directories modularly to allow managing hundreds of virtual hosts cleanly.

```text
/etc/nginx/
├── nginx.conf                 # Primary entry point
├── conf.d/                    # Drop-in configuration directory
│   ├── default.conf
│   └── telemetry.conf
├── sites-available/           # Full site configurations (Debian/Ubuntu pattern)
│   ├── api.example.com.conf
│   └── app.example.com.conf
├── sites-enabled/             # Symlinks pointing to sites-available/
│   └── api.example.com.conf -> /etc/nginx/sites-available/api.example.com.conf
└── mime.types                 # Mapping of file extensions to MIME types
```

### Clean `nginx.conf` Boilerplate
```nginx
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
    worker_connections 8192;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for" '
                    'rt=$request_time uct="$upstream_connect_time" uht="$upstream_header_time" urt="$upstream_response_time"';

    access_log /var/log/nginx/access.log main;

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    # Include all modular configurations
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Event Driven Architecture and Worker Model](./01-Event-Driven-Architecture-and-Worker-Model.md) | [Index](../../../README.md) | [03 - Location Block Matching Priority and Directives →](./03-Location-Block-Matching-Priority-and-Directives.md) |
