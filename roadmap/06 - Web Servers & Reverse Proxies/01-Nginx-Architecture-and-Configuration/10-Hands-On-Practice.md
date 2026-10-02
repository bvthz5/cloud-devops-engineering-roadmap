# 10 - Hands-On Practice Labs

## Lab 1: Multi-Tenant Virtual Host with Hardened Default Catch-All

### Objective
Configure Nginx with a secure default catch-all server block that drops unknown host header requests, and create two distinct virtual hosts with separate logging.

### Implementation (`/etc/nginx/conf.d/vhosts.conf`)
```nginx
# 1. Default Catch-All (Drops illegal host headers)
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name _;
    return 444;
}

# 2. Main Web Application
server {
    listen 80;
    server_name app.example.com;

    access_log /var/log/nginx/app_access.log main;
    error_log /var/log/nginx/app_error.log warn;

    root /var/www/app;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# 3. Microservice API Route
server {
    listen 80;
    server_name api.example.com;

    access_log /var/log/nginx/api_access.log main;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
