# 10 - Hands-On Practice Labs

## Lab 1: Multi-Tier Resilient Load Balancer with Failover

### Objective
Configure an Nginx reverse proxy with a weighted upstream pool, passive health checking with automatic failover, and keepalive pooling.

### Implementation
```nginx
upstream api_cluster {
    least_conn;

    server 10.0.1.10:8080 weight=3 max_fails=2 fail_timeout=10s;
    server 10.0.1.11:8080 weight=1 max_fails=2 fail_timeout=10s;
    server 10.0.1.99:8080 backup;

    keepalive 32;
}

server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://api_cluster;
        proxy_http_version 1.1;
        proxy_set_header Connection "";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_next_upstream error timeout http_502 http_503;
        proxy_connect_timeout 2s;
        proxy_read_timeout 15s;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
