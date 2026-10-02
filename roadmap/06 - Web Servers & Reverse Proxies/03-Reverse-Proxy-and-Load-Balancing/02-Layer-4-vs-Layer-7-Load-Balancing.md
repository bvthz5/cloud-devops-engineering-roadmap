# 02 - Layer 4 (TCP/UDP) vs Layer 7 (HTTP) Load Balancing

## 1. Layer 4 vs Layer 7 Comparison

```text
+-----------------------------------------------------------------------------------+
|                        OSI Layer Balancing Comparison                             |
+-------------------+-------------------------------+-------------------------------+
| Attribute         | Layer 4 (Transport / TCP/UDP) | Layer 7 (Application / HTTP)  |
+-------------------+-------------------------------+-------------------------------+
| Inspection Depth  | IP address and TCP/UDP port   | Full HTTP headers, cookies,   |
|                   | only. Zero payload parsing.   | URL path, body JSON, gRPC.    |
+-------------------+-------------------------------+-------------------------------+
| Performance       | Ultra-high throughput,        | Moderate throughput, higher   |
|                   | minimal CPU, low latency.     | CPU overhead for parsing.     |
+-------------------+-------------------------------+-------------------------------+
| TLS Termination   | Passthrough or SNI routing.   | Full TLS handshake, header    |
|                   | Cannot read encrypted data.   | injection, cookie inspection. |
+-------------------+-------------------------------+-------------------------------+
| Routing Logic     | Simple IP/Port forwarding.    | Path-based routing (/api,     |
|                   |                               | /auth), header-based routing. |
+-------------------+-------------------------------+-------------------------------+
```

---

## 2. Nginx Layer 4 (Stream) vs Layer 7 (HTTP) Configuration

### 2.1 Layer 4 TCP Stream Balancing (e.g., MySQL or Redis Cluster)
Configured inside the top-level `stream {}` context (outside `http {}`):

```nginx
# /etc/nginx/nginx.conf
stream {
    upstream db_cluster {
        server 10.0.2.10:3306 max_fails=3 fail_timeout=10s;
        server 10.0.2.11:3306 max_fails=3 fail_timeout=10s;
    }

    server {
        listen 3306;
        proxy_pass db_cluster;
        proxy_connect_timeout 2s;
        proxy_timeout 10m;
    }
}
```

### 2.2 Layer 7 HTTP Application Balancing
```nginx
http {
    upstream web_cluster {
        server 10.0.1.10:8080;
        server 10.0.1.11:8080;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://web_cluster;
        }
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Forward Proxy vs Reverse Proxy Architecture](./01-Forward-Proxy-vs-Reverse-Proxy-Architecture.md) | [Index](../../../README.md) | [03 - Load Balancing Algorithms →](./03-Load-Balancing-Algorithms.md) |
