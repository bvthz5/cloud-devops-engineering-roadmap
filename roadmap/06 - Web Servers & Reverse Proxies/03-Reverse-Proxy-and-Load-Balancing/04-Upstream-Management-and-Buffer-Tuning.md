# 04 - Upstream Management and Buffer Tuning

## 1. Upstream Keepalive Connection Pools

By default, Nginx opens a new TCP connection to the upstream backend for **every single incoming client request** and closes it immediately after receiving the response! In high-traffic environments, this creates extreme TCP handshake overhead and socket exhaustion (`TIME_WAIT` pileups).

```nginx
upstream backend_service {
    server 10.0.1.10:8080;
    server 10.0.1.11:8080;

    # Maintain up to 64 idle keepalive connections per worker to upstream
    keepalive 64;
    keepalive_requests 1000;
    keepalive_timeout 60s;
}

server {
    listen 80;
    
    location / {
        proxy_pass http://backend_service;
        
        # MANDATORY for HTTP/1.1 keepalive to upstream:
        proxy_http_version 1.1;
        proxy_set_header Connection ""; # Clear 'close' header!
    }
}
```

---

## 2. Proxy Buffer Tuning

Nginx buffers upstream responses in memory so the backend can release its worker thread immediately, even if the external client is reading over a slow 3G mobile connection.

```nginx
location / {
    proxy_pass http://backend_service;

    # Enable response buffering
    proxy_buffering on;

    # Size of the buffer used for reading first part of response (headers)
    proxy_buffer_size 8k;

    # Number and size of buffers for response body
    proxy_buffers 16 16k;

    # Total buffer size that can be busy sending to client
    proxy_busy_buffers_size 32k;

    # Maximum size of temporary file written to disk if buffers overflow
    proxy_max_temp_file_size 1024m;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Load Balancing Algorithms](./03-Load-Balancing-Algorithms.md) | [README](./README.md) | [05 - Active vs Passive Health Checking](./05-Active-vs-Passive-Health-Checking.md) |
