# 🌐 Web Servers & Reverse Proxies — Complete Kids-Mind Deep Dive Guide

> **Format:** Simple 1-line definitions, real-life analogies, visual traffic diagrams, and production-ready configuration templates for NGINX, Apache, HAProxy, Traefik, Caddy, and Envoy.

---

## 📜 1. History of Web Servers & Reverse Proxies: Who, When, & What Crisis Sparked the Invention?

| Technology / Milestone | Inventor(s) & Organization | Year | The Real Problem / Crisis That Triggered It | Kids-Mind Analogy |
| :--- | :--- | :--- | :--- | :--- |
| **CERN httpd & NCSA HTTPd** | **Tim Berners-Lee** (CERN, 1990) / **Rob McCool** (NCSA, 1993) | **1990 / 1993** | The birth of the web. Early academic sites needed a background daemon to listen on port 80 and stream static HTML files over TCP sockets to Mosaic browsers. | The very first vending machine dispensing physical paper copies of university pamphlets. |
| **Apache HTTP Server** | **Brian Behlendorf, Roy Fielding, Robert McCool** (The Apache Group) | **1995** | NCSA development stalled when key developers left to build Netscape. Webmasters around the world had written disparate bug fixes and features. Behlendorf and colleagues collected these patches to create "A PAtCHy Server" (Apache), which dominated 60%+ of the global web for 15 years. | A community cooperative repairing and upgrading a communal tractor with donated spare parts. |
| **The C10K Problem & NGINX** | **Igor Sysoev** (Rambler / NGINX) | **2002–2004** | **The C10K Problem**: Apache created a separate heavy thread or process for every single user connection. When 10,000 users connected simultaneously, the server ran out of RAM and CPU choked in context switching. Sysoev designed NGINX using an **asynchronous, non-blocking event loop (`epoll`/`kqueue`)**, allowing a single process to handle 50,000+ concurrent connections on minimal RAM. | A master juggler keeping 50 balls in the air at once instead of hiring 50 separate people to hold one ball each. |
| **HAProxy (High Availability Proxy)** | **Willy Tarreau** | **2001** | High-traffic websites crashed when a single web server died, and early software load balancers dropped packets under heavy traffic. Tarreau wrote HAProxy in C with event-driven I/O for lightning-fast layer 4/7 traffic distribution and health-checking. | A super-fast traffic police officer routing cars evenly across 10 open highway toll lanes. |
| **Traefik (Cloud-Native Edge Router)** | **Émile Vauge** (Containous / Traefik Labs) | **2015** | In Docker and Kubernetes microservices, containers appear and disappear every few seconds with dynamic IP addresses. Traditional NGINX required rewriting static configuration files and reloading the daemon. Vauge built Traefik to listen directly to the Docker/K8s API and automatically generate routing rules on the fly with zero reloads. | A magical airport departure board that updates gates instantly the moment a plane lands without needing a human painter. |
| **Caddy (Automatic HTTPS Web Server)** | **Matt Holt** | **2015** | Configuring SSL/TLS certificates with OpenSSL and setting up `cron` renewal scripts was complex and error-prone; expired certificates constantly broke websites. Holt wrote Caddy in Go with memory safety and automatic out-of-the-box Let's Encrypt TLS certificate provisioning. | A house that automatically manufactures and installs its own unbreakable high-security deadbolt lock the moment you build it. |
| **Envoy Proxy (Service Mesh Data Plane)** | **Matt Klein** (Lyft) | **2016** | Microservices at Lyft were failing due to hidden network timeouts, cascading crashes, and lack of visibility. Klein built Envoy in C++ as an out-of-process sidecar proxy with dynamic APIs (xDS), advanced load balancing, and distributed tracing. | An invisible personal bodyguard walking beside each soldier with a high-speed walkie-talkie and automated shield. |

### 🕰️ Visual Timeline: The Evolution of Web Servers

```text
 1990/1993 ──── 1995 ──────── 1999 ──────── 2001 ────── 2004 ────── 2015/2016 ──── Present
     │            │             │            │           │           │                 │
 CERN / NCSA    Apache      Dan Kegel     HAProxy      NGINX     Traefik, Caddy     Envoy, Istio,
 Earliest      "A Patchy    C10K Problem  Willy        Igor      & Envoy Proxy      Cloudflare Edge,
 Static Web    Server"      Concurrency   Tarreau      Sysoev    Containers &       API Gateways &
 Daemons       Dominance    Challenge     TCP/HTTP L7  EventLoop Cloud-Native       Service Meshes
```

---

## 🧭 2. Web Server vs. Reverse Proxy vs. Forward Proxy (Deep Dive)

```text
FORWARD PROXY (Sits in front of CLIENTS - Hides the Client):
┌──────────────┐      ┌──────────────────────────┐      ┌─────────────────────────┐
│ Client 1     │──┐   │ Forward Proxy            │      │ Public Internet         │
│ (10.0.1.5)   │  └──►│ (Corporate / School)     │─────►│ (google.com / github)   │
├──────────────┤      │ • Enforces filters       │      │                         │
│ Client 2     │─────►│ • Caches shared files    │      │ Only sees Proxy Public  │
│ (10.0.1.6)   │      │ • Hides client IP        │      │ IP: 203.0.113.50        │
└──────────────┘      └──────────────────────────┘      └─────────────────────────┘

REVERSE PROXY (Sits in front of SERVERS - Hides the Backend):
┌─────────────────────────┐      ┌───────────────────────┐      ┌─────────────────────────┐
│ Public Visitors         │      │ Reverse Proxy         │      │ Private Backend Servers │
│ (Worldwide Internet)    │      │ (NGINX / HAProxy / ALB│      │ (AWS VPC / Docker LAN)  │
├─────────────────────────┤      ├───────────────────────┤      ├─────────────────────────┤
│ https://example.com     │─────►│ Port 80 / 443 Public  │──┬──►│ Node.js API (:3000)     │
│                         │      │ • SSL Termination     │  ├──►│ Python Flask (:5000)    │
│ Users never see backend │      │ • Load Balancing      │  └──►│ Static Files (/var/www) │
│ private IP addresses!   │      │ • DDoS Protection     │      │                         │
└─────────────────────────┘      └───────────────────────┘      └─────────────────────────┘
```

### Master Comparison Table:

| Feature | Web Server | Reverse Proxy | Forward Proxy |
| :--- | :--- | :--- | :--- |
| **Where it Sits** | Directly hosts and serves website files. | In front of internal backend servers. | In front of internal client devices/employees. |
| **Who it Represents** | The web application itself. | **The destination servers** (hides backends). | **The client devices** (hides clients). |
| **Primary Job** | Reads files off disk, returns HTML/CSS/JS. | Routes requests, terminates SSL, load balances. | Filters content, enforces security, monitors egress. |
| **Client Awareness** | Client knows they are contacting this server. | Client thinks the reverse proxy IS the server! | Client explicitly configures proxy settings in browser. |
| **Caching** | Caches local static files in memory. | Caches dynamic API responses & microcaching. | Caches internet downloads to save office bandwidth. |
| **Kids-Mind Analogy** | A chef cooking meals and handing them out. | The restaurant receptionist greeting guests. | A school chaperone escorting children on a trip. |
| **Top Examples** | Apache, NGINX, Caddy, Lighttpd. | NGINX, HAProxy, Traefik, AWS ALB, Cloudflare. | Squid, Envoy Egress, Zscaler, Blue Coat. |

---

## ⚡ 3. How Web Servers Work Internally: The C10K Problem & Concurrency Models

When a browser opens `https://example.com`, what happens under the hood?

```text
1. Browser resolves DNS ➔ Obtains IP (e.g. 54.210.10.5).
2. TCP 3-Way Handshake ➔ SYN ➔ SYN-ACK ➔ ACK to Port 443.
3. TLS Handshake ➔ Client Hello ➔ Certificate Exchange ➔ Symmetric Key Derived.
4. HTTP Request Sent ➔ "GET /products HTTP/1.1\r\nHost: example.com\r\n\r\n".
5. Web Server processes request via one of two concurrency models:
```

### 3.1 Process-Per-Connection (Apache Prefork MPM) vs. Event-Driven (NGINX)

```text
APACHE PREFORK (Process / Thread per Connection):
Client 1 ──► [ Process 1 ] (Consumes ~2-5 MB RAM)
Client 2 ──► [ Process 2 ] (Consumes ~2-5 MB RAM)
Client 3 ──► [ Process 3 ] (Consumes ~2-5 MB RAM)
...
Client 10,000 ──► Out of RAM! CPU thrashing in context switching! 💥 (The C10K Crash)

NGINX EVENT-DRIVEN (Asynchronous Event Loop with epoll):
Client 1 ──────┐
Client 2 ──────┼──► [ epoll() Kernel Queue ] ──► [ Single NGINX Worker Process ]
Client 3 ──────┤     (Notifies worker only         (Handles 10,000+ active sockets
Client 10,000 ─┘      when data is ready!)          using only ~20 MB of RAM!) 🚀
```

- **Why NGINX won the web:** Instead of creating a heavy thread that sleeps while waiting for a slow client to send bytes, NGINX uses Linux kernel **`epoll`** (or BSD **`kqueue`**). The operating system wakes up the NGINX worker only when a network packet arrives.

---

## 🛠️ 4. NGINX Master Architecture & Production Configuration

NGINX runs as **one master process** and **multiple worker processes**:
- **Master Process (runs as root):** Reads configuration files, binds to privileged ports (80, 443), and manages worker lifecycles.
- **Worker Processes (run as unprivileged `www-data` or `nginx`):** Perform network I/O, evaluate routing, terminate SSL, and proxy upstream requests.

```text
                        ┌───────────────────────────────┐
                        │   Master Process (PID 1204)   │
                        │   (Reads nginx.conf as root)   │
                        └───────────────┬───────────────┘
                                        │ Spawns 1 worker per CPU core
            ┌───────────────────────────┼───────────────────────────┐
            ▼                           ▼                           ▼
┌───────────────────────┐   ┌───────────────────────┐   ┌───────────────────────┐
│ Worker 1 (CPU Core 0) │   │ Worker 2 (CPU Core 1) │   │ Worker 3 (CPU Core 2) │
│ epoll event loop      │   │ epoll event loop      │   │ epoll event loop      │
└───────────────────────┘   └───────────────────────┘   └───────────────────────┘
```

### 4.1 Production Master `nginx.conf` with All Enterprise Best Practices

```nginx
# /etc/nginx/nginx.conf

# ==============================================================================
# 1. MAIN GLOBAL CONTEXT
# ==============================================================================
user www-data;
worker_processes auto;                  # Automatically spawns 1 worker per CPU core
pid /run/nginx.pid;
worker_rlimit_nofile 65535;             # Increases OS file descriptor limit per worker

# ==============================================================================
# 2. EVENTS CONTEXT (Connection Handling)
# ==============================================================================
events {
    worker_connections 4096;            # Max simultaneous connections per worker
    use epoll;                          # Linux high-performance event notification
    multi_accept on;                    # Worker accepts all new connections at once
}

# ==============================================================================
# 3. HTTP CONTEXT (All Web & Reverse Proxy Directives)
# ==============================================================================
http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    # Performance Optimizations
    sendfile on;                        # Zero-copy kernel file transfer
    tcp_nopush on;                      # Sends HTTP headers in one packet
    tcp_nodelay on;                     # Disables Nagle's algorithm for instant latency
    keepalive_timeout 65;               # Keeps TCP connection open for subsequent assets
    types_hash_max_size 2048;
    server_tokens off;                  # HIDES NGINX VERSION FOR SECURITY!

    # Client Request Limits (DDoS Protection)
    client_max_body_size 20M;           # Max upload file size (prevents 413 error)
    client_body_buffer_size 128k;
    client_header_buffer_size 1k;

    # Gzip Compression
    gzip on;
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml image/svg+xml;

    # Rate Limiting Zones (Token Bucket Algorithm)
    limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;
    limit_conn_zone $binary_remote_addr zone=conn_limit:10m;

    # Microcache Path Setup
    proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m max_size=1g inactive=60m use_temp_path=off;

    # ==========================================================================
    # 4. UPSTREAM CONTEXT (Backend Server Cluster & Load Balancing)
    # ==========================================================================
    upstream backend_cluster {
        least_conn;                     # Route to server with fewest active connections
        server 10.0.1.10:3000 weight=3 max_fails=3 fail_timeout=10s;
        server 10.0.1.11:3000 weight=2 max_fails=3 fail_timeout=10s;
        server 10.0.1.12:3000 backup;   # Standby failover server!
        keepalive 32;                   # Maintain persistent TCP pool to upstream
    }

    # ==========================================================================
    # 5. SERVER BLOCK: HTTP to HTTPS Redirect
    # ==========================================================================
    server {
        listen 80;
        listen [::]:80;
        server_name example.com www.example.com;

        # Redirect all cleartext HTTP to secure HTTPS
        return 301 https://$host$request_uri;
    }

    # ==========================================================================
    # 6. SERVER BLOCK: Secure Production HTTPS Virtual Host
    # ==========================================================================
    server {
        listen 443 ssl http2;
        listen [::]:443 ssl http2;
        server_name example.com www.example.com;

        # SSL Certificates & Security Hardening
        ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256;
        ssl_session_cache shared:SSL:10m;
        ssl_session_timeout 1d;
        ssl_session_tickets off;

        # Security Headers
        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
        add_header X-Frame-Options "SAMEORIGIN" always;
        add_header X-Content-Type-Options "nosniff" always;
        add_header X-XSS-Protection "1; mode=block" always;
        add_header Referrer-Policy "no-referrer-when-downgrade" always;

        # Root Folder for Static Assets (React/Vue/Static HTML)
        root /var/www/my-frontend;
        index index.html;

        # ----------------------------------------------------------------------
        # Location 1: Frontend SPA Routing (Single Page Applications)
        # ----------------------------------------------------------------------
        location / {
            try_files $uri $uri/ /index.html;
        }

        # ----------------------------------------------------------------------
        # Location 2: Static Assets Caching
        # ----------------------------------------------------------------------
        location ~* \.(jpg|jpeg|png|gif|ico|css|js|woff2)$ {
            expires 30d;
            add_header Cache-Control "public, no-transform";
            access_log off;
        }

        # ----------------------------------------------------------------------
        # Location 3: Reverse Proxy for Dynamic API Requests
        # ----------------------------------------------------------------------
        location /api/ {
            # Enforce Rate Limiting (Burst 20, no delay)
            limit_req zone=api_limit burst=20 nodelay;
            limit_conn conn_limit 10;

            # Proxy Pass to Upstream Pool
            proxy_pass http://backend_cluster;
            proxy_http_version 1.1;

            # Essential Proxy Headers (Preserve Client Identity)
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # WebSocket Support
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";

            # Proxy Timeouts & Buffer Tuning
            proxy_connect_timeout 5s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
            proxy_buffers 8 16k;
            proxy_buffer_size 32k;

            # Microcaching Setup
            proxy_cache my_cache;
            proxy_cache_valid 200 302 10m;
            proxy_cache_valid 404 1m;
            add_header X-Cache-Status $upstream_cache_status; # Displays HIT, MISS, BYPASS
        }

        # ----------------------------------------------------------------------
        # Location 4: Custom Error Pages
        # ----------------------------------------------------------------------
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
            root /usr/share/nginx/html;
        }
    }
}
```

---

## ⚖️ 5. Load Balancing Algorithms Explained Simply

| Algorithm | How It Works (Kids Mind) | Real-World When to Use |
| :--- | :--- | :--- |
| **Round Robin (Default)** | Distributes requests in a rigid rotating circle: Server 1 ➔ Server 2 ➔ Server 3 ➔ Server 1. | When backend servers have identical CPU/RAM specs and requests require uniform computation. |
| **Least Connections (`least_conn`)** | Checks active socket count and directs traffic to the server currently handling the fewest requests. | **Best for production APIs!** Prevents one server from choking on slow database queries or WebSockets. |
| **IP Hash (`ip_hash`)** | Hashes client IP to guarantee requests from the same user always land on the exact same server. | Legacy stateful applications where user session data is kept in server local RAM rather than Redis. |
| **Generic Hash (`hash $request_uri`)** | Hashes a specific variable (like URL or session cookie) for consistent caching affinity. | Upstream API caches where caching is vastly more efficient when identical URLs hit the same node. |
| **Weighted (`weight=N`)** | Allocates traffic proportionally (e.g., Server A with `weight=3` gets 75% of traffic, Server B gets 25%). | During hardware migrations when blending massive 64-core servers with older 16-core nodes. |

---

## 🪶 6. Apache HTTP Server (httpd) Deep Dive

Apache was the undisputed king of web servers for over a decade. Understanding its configuration and Multi-Processing Modules (MPMs) is essential for enterprise DevOps.

### 6.1 Apache Multi-Processing Modules (MPMs):
1. **`mpm_prefork` (Non-threaded):** Spawns multiple isolated child processes. Each process handles 1 connection.
   - *Advantage:* Safe for thread-unsafe libraries (legacy PHP).
   - *Disadvantage:* Consumes huge RAM; cannot scale under high traffic.
2. **`mpm_worker` (Multi-threaded):** Spawns child processes, each containing multiple worker threads.
   - *Advantage:* Far less RAM consumption than prefork.
3. **`mpm_event` (Modern Hybrid):** Similar to worker, but dedicates a listener thread to handle Keep-Alive connections asynchronously.
   - *Advantage:* Apache's highest performance mode.

### 6.2 `.htaccess` vs. Main Configuration (The Performance Trap)
- **What is `.htaccess`?** A configuration file placed inside website directories allowing non-admin users to change rewrite rules without root access.
- **Why DevOps engineers disable `.htaccess` in production (`AllowOverride None`):** When enabled, Apache must traverse the disk and search for `.htaccess` in **every single parent folder** for **every single HTTP request**, crippling performance!

### 6.3 Apache Production Reverse Proxy Snippet:
```apache
<VirtualHost *:443>
    ServerName api.example.com
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/api.crt
    SSLCertificateKeyFile /etc/ssl/private/api.key

    # Reverse Proxy to Backend Node.js
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:3000/
    ProxyPassReverse / http://127.0.0.1:3000/

    # Security Headers
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "SAMEORIGIN"
</VirtualHost>
```

---

## 🚦 7. HAProxy (High Availability Proxy) Deep Dive

HAProxy is dedicated purely to **ultra-fast Layer 4 (TCP) and Layer 7 (HTTP) load balancing**. It does not serve static HTML files from disk; it exists to route packets at near-wire speeds.

```text
┌────────────────────────────────────────────────────────┐
│ HAProxy Architecture:                                  │
│ • global: Process settings (user, group, maxconn, log) │
│ • defaults: Default timeout and mode settings          │
│ • frontend: Inbound listener (ports, SSL, ACL rules)   │
│ • backend: Target servers, health checks, balance type │
└────────────────────────────────────────────────────────┘
```

### 7.1 HAProxy Production `haproxy.cfg` Configuration:
```haproxy
global
    log /dev/log local0
    user haproxy
    group haproxy
    daemon
    maxconn 50000

defaults
    log     global
    mode    http
    option  httplog
    option  dontlognull
    timeout connect 5000ms
    timeout client  50000ms
    timeout server  50000ms

# Public Frontend
frontend https_in
    bind *:443 ssl crt /etc/ssl/certs/site.pem
    mode http
    option forwardfor
    
    # Access Control Lists (ACLs)
    acl is_api path_beg /api/
    use_backend api_cluster if is_api
    default_backend web_cluster

# Backend Cluster 1: API Servers
backend api_cluster
    mode http
    balance roundrobin
    option httpchk GET /health
    server api1 10.0.1.20:5000 check inter 2000ms rise 2 fall 3
    server api2 10.0.1.21:5000 check inter 2000ms rise 2 fall 3

# Backend Cluster 2: Static Web
backend web_cluster
    mode http
    balance leastconn
    cookie SRV insert indirect nocache
    server web1 10.0.1.10:80 check cookie s1
    server web2 10.0.1.11:80 check cookie s2

# Built-in Live Statistics Dashboard
listen stats
    bind *:8404
    stats enable
    stats uri /
    stats refresh 10s
    stats admin if TRUE
```

---

## 🚀 8. Modern Cloud-Native Reverse Proxies: Traefik, Caddy & Envoy

| Feature | Traefik | Caddy | Envoy Proxy |
| :--- | :--- | :--- | :--- |
| **Primary Strength** | **Automatic container service discovery** via Docker/K8s labels. | **Zero-config automatic HTTPS** and simple Caddyfile. | **L3/L4/L7 service mesh data plane** with dynamic xDS APIs. |
| **Written In** | Go | Go | C++ |
| **Dynamic Reconfig** | Listens to Docker/Kubernetes API without restarts. | REST API + Dynamic JSON. | Full control plane xDS dynamic streaming. |
| **Best Used In** | Docker Compose microservices, K8s Ingress. | Developer environments, quick HTTPS setups. | Istio Service Mesh, high-scale microservices. |

### 8.1 Traefik in Docker Compose (Zero NGINX Config Needed!):
```yaml
version: "3.8"
services:
  reverse-proxy:
    image: traefik:v3.0
    command:
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--entrypoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080" # Traefik Web UI
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

  my-app:
    image: node:20-alpine
    labels:
      # Traefik automatically discovers this container and routes traffic!
      - "traefik.enable=true"
      - "traefik.http.routers.myapp.rule=Host(`api.example.com`)"
      - "traefik.http.services.myapp.loadbalancer.server.port=3000"
```

### 8.2 Caddy (`Caddyfile` - Complete HTTPS in 3 Lines!):
```caddy
example.com {
    reverse_proxy 127.0.0.1:3000
    encode gzip zstd
}
```
*(Caddy automatically contacts Let's Encrypt, generates SSL certs, handles HTTP-01 challenges, and renews certs in background!)*

---

## 🔒 9. Essential HTTP Security Headers & WAF Cheat Sheet

```text
┌───────────────────────────────────────┬────────────────────────────────────────────────────────┐
│ Header Name                           │ What It Protects & Recommended Value                   │
├───────────────────────────────────────┼────────────────────────────────────────────────────────┤
│ Strict-Transport-Security (HSTS)      │ Forces HTTPS connection; prevents SSL stripping.       │
│                                       │ max-age=31536000; includeSubDomains; preload           │
├───────────────────────────────────────┼────────────────────────────────────────────────────────┤
│ X-Frame-Options                       │ Prevents Clickjacking by disallowing invisible iframes.│
│                                       │ DENY or SAMEORIGIN                                     │
├───────────────────────────────────────┼────────────────────────────────────────────────────────┤
│ X-Content-Type-Options                │ Prevents MIME-sniffing malicious executable uploads.   │
│                                       │ nosniff                                                │
├───────────────────────────────────────┼────────────────────────────────────────────────────────┤
│ Content-Security-Policy (CSP)         │ Restricts script/style execution domains (Stops XSS).  │
│                                       │ default-src 'self'; script-src 'self' cdn.example.com  │
├───────────────────────────────────────┼────────────────────────────────────────────────────────┤
│ Referrer-Policy                       │ Controls URL leaking in Referer header to external APIs│
│                                       │ strict-origin-when-cross-origin                        │
├───────────────────────────────────────┼────────────────────────────────────────────────────────┤
│ Permissions-Policy                    │ Disables browser camera, microphone, and geolocation.  │
│                                       │ camera=(), microphone=(), geolocation=()               │
└───────────────────────────────────────┴────────────────────────────────────────────────────────┘
```

---

## 🚨 10. Reverse Proxy Troubleshooting & Incident Playbook

When an upstream reverse proxy fails, follow this diagnostic decision tree:

```text
Error Code ──► 502 Bad Gateway
  • Cause: Reverse proxy reached the backend IP, but NO PROCESS WAS LISTENING or connection was refused.
  • Check:
    1. Is the backend process running? $ pgrep -a node / ps aux
    2. Is it bound to localhost instead of 0.0.0.0 inside a container?
    3. Did the backend crash on startup? $ docker logs <container>

Error Code ──► 504 Gateway Timeout
  • Cause: Reverse proxy connected, but backend took longer than proxy_read_timeout (default 60s) to respond.
  • Check:
    1. Backend database deadlock or unindexed slow SQL query.
    2. Upstream microservice network latency or infinite loop.

Error Code ──► 403 Forbidden
  • Cause: File permissions issue or directory listing disabled without an index.html.
  • Fix: $ chmod 755 /var/www/html && chmod 644 /var/www/html/*

Error Code ──► 413 Request Entity Too Large
  • Cause: File upload exceeds NGINX client_max_body_size (default 1MB).
  • Fix: Add client_max_body_size 50M; inside http or server block.
```

### Essential Linux Commands for Web Server Debugging:
```bash
# 1. Test NGINX configuration syntax before restarting!
sudo nginx -t

# 2. Safely reload NGINX without dropping active user connections!
sudo systemctl reload nginx

# 3. Stream live access logs with HTTP response status codes
tail -f /var/log/nginx/access.log | awk '{print $7, $9}'

# 4. Stream recent error logs
sudo journalctl -u nginx -n 50 --no-pager
```

---

## 🎯 11. Real-World Production Incident: Post-Deployment 502 Bad Gateway Triage

**The Incident:** DevOps deploys a new version of the frontend and API using Docker and NGINX. Five seconds later, monitoring fires alerts: *"100% of external visitors receive 502 Bad Gateway!"*

### Step-by-Step Triage & Resolution:
1. **Verify NGINX Error Log:**
   `$ sudo tail -n 20 /var/log/nginx/error.log`
   ``text
   [error] 1240#1240: *1 connect() failed (111: Connection refused) while connecting to upstream, client: 198.51.100.4, server: example.com, request: "GET /api/v1/users HTTP/1.1", upstream: "http://127.0.0.1:3000/api/v1/users"
   ``
2. **Inspect Sockets on Host:**
   `$ sudo ss -tulpn | grep 3000`
   Output shows: `LISTEN 0 128 127.0.0.1:3000`
3. **The Root Cause:**
   The backend container was started with Docker port mapping `-p 3000:3000`, but the Node.js app code had:
   `app.listen(3000, 'localhost')` inside the container!
   *Remember: `localhost` inside a Docker container refers only to the container itself, NOT the host or Docker bridge!*
4. **The Fix:**
   Updated Node.js to bind to `0.0.0.0` (all interfaces):
   `app.listen(3000, '0.0.0.0')`
   Reloaded containers. Traffic instantly resumed with `200 OK`!

---

## ❓ 12. Quick Revision Quiz & Interview Questions

1. **What is the difference between a forward proxy and a reverse proxy?**
   - *Answer:* A forward proxy sits in front of client devices to protect, filter, or anonymize outgoing internet requests. A reverse proxy sits in front of backend servers to load balance, terminate SSL, and protect internal infrastructure.
2. **Why does NGINX scale better than Apache under high concurrent load?**
   - *Answer:* NGINX uses an asynchronous, event-driven event loop (`epoll`/`kqueue`) where one worker handles thousands of connections with minimal RAM. Classic Apache creates a separate thread/process per connection, exhausting memory and CPU in context switching (the C10K problem).
3. **What is SSL Termination and why is it done on a reverse proxy?**
   - *Answer:* SSL Termination is the process of decrypting HTTPS traffic at the reverse proxy edge. The proxy handles CPU-heavy cryptographic handshakes, allowing internal backend services to communicate over fast HTTP/TCP on an isolated private network.
4. **What causes an HTTP 502 Bad Gateway vs HTTP 504 Gateway Timeout?**
   - *Answer:* 502 Bad Gateway means the reverse proxy could not connect to the backend (service down, wrong port, refused connection). 504 Gateway Timeout means the proxy connected, but the backend took too long to return data.
5. **Why should `server_tokens off;` be enabled in NGINX?**
   - *Answer:* It hides the specific NGINX version number from HTTP response headers, preventing attackers from identifying unpatched CVE vulnerabilities.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Azure Services & Types Short Notes](./13-Azure-Services-and-Types-Deep-Dive-Short-Notes.md) | [Index](../README.md) | [Monitoring, Logging & Observability Short Notes →](./15-Monitoring-Logging-and-Observability-Short-Notes.md) |
