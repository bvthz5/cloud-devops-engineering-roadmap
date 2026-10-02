# 4. Nginx Reverse Proxy Configuration

## What is a Reverse Proxy?

A reverse proxy sits in front of backend servers, receiving client requests and forwarding them to internal application servers (Node.js, Python Gunicorn, Java Tomcat, Go).

```text
[ Client ] ──── HTTPS (443) ────► [ Nginx Reverse Proxy ] ──── HTTP (5000) ────► [ App Server ]
                                 (TLS Termination,      (Internal Private
                                  Caching, Hardening)    Network Connection)
```

## Production Reverse Proxy Configuration

```nginx
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://127.0.0.1:5000;
        
        # Mandatory Header Forwarding
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # HTTP Version & Keep-Alive Tuning
        proxy_http_version 1.1;
        proxy_set_header Connection "";

        # Timeouts & Buffering
        proxy_connect_timeout 60s;
        proxy_read_timeout 60s;
        proxy_send_timeout 60s;
        proxy_buffers 16 4k;
        proxy_buffer_size 2k;
    }
}
```

## Crucial Proxy Directives Explained
- `proxy_pass`: Directs traffic to specified backend target (URL, IP, or Upstream block).
- `X-Real-IP`: Passes real client IPv4/IPv6 address to upstream application logs.
- `X-Forwarded-For`: Appends client and proxy IP chain so upstream knows full hop path.
- `X-Forwarded-Proto`: Informs backend whether request originally arrived over `http` or `https`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Nginx Configuration Structure and Server Blocks](./03-Nginx-Configuration-Structure-and-Server-Blocks.md) | [README](./README.md) | [05 - Nginx Load Balancing Algorithms and Health Checks](./05-Nginx-Load-Balancing-Algorithms-and-Health-Checks.md) |
