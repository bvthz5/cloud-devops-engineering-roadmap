# 12 - Quick-Revision & Enterprise Cheat Sheet

```nginx
# Upstream Keepalive Pool Boilerplate
upstream backend {
    server 10.0.1.10:8080 max_fails=3 fail_timeout=10s;
    server 10.0.1.11:8080 max_fails=3 fail_timeout=10s;
    keepalive 64;
}

# Proxy Pass Mandatory Headers
location / {
    proxy_pass http://backend;
    proxy_http_version 1.1;
    proxy_set_header Connection "";
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}

# Passive Failover
proxy_next_upstream error timeout http_502 http_503;
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (04-SSL-TLS-Certificates-and-HTTPS) →](../04-SSL-TLS-Certificates-and-HTTPS/01-TLS-Handshake-Architecture-1.2-vs-1.3.md) |
