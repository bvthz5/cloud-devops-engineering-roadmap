# 10 - Hands-On Practice: Configuring Production NGINX Load Balancing

## Lab Scenario
Deploy NGINX as a Layer 7 reverse proxy balancing traffic across two backend web servers with weighted round robin and health monitoring.

```nginx
# /etc/nginx/conf.d/load_balancer.conf

upstream web_cluster {
    # Server 1: Primary worker with double capacity
    server 10.0.1.10:8080 weight=2 max_fails=3 fail_timeout=10s;

    # Server 2: Secondary worker
    server 10.0.1.11:8080 weight=1 max_fails=3 fail_timeout=10s;

    # Keepalive connections to backend pool
    keepalive 32;
}

server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://web_cluster;

        # Standard proxy headers
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Enable HTTP/1.1 for keepalive upstream support
        proxy_http_version 1.1;
        proxy_set_header Connection "";

        # Timeouts
        proxy_connect_timeout 5s;
        proxy_read_timeout 60s;
        proxy_send_timeout 60s;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
