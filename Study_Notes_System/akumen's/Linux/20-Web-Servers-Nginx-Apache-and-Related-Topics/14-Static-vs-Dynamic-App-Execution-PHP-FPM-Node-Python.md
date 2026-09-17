# 14. Static vs Dynamic Execution: PHP-FPM, Node.js, Python WSGI/ASGI

## Architecture Patterns Overview

Web servers cannot execute dynamic programming languages directly in process safely at high concurrency. They delegate execution to dedicated application gateways or runtimes:

```text
1. PHP-FPM (FastCGI Protocol):
   Nginx ── (FastCGI / unix:/run/php/php8.2-fpm.sock) ──► PHP-FPM Master/Worker

2. Node.js (Reverse Proxy):
   Nginx ── (HTTP / http://127.0.0.1:3000) ──► Node.js Event Loop (Express/NestJS)

3. Python WSGI/ASGI (Reverse Proxy):
   Nginx ── (HTTP / http://127.0.0.1:8000) ──► Gunicorn / Uvicorn (Django/FastAPI)
```

## 1. Nginx + PHP-FPM Configuration
```nginx
location ~ \.php$ {
    include fastcgi_params;
    fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_index index.php;
}
```

## 2. Nginx + Node.js Configuration
```nginx
location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
}
```

## 3. Nginx + Python Gunicorn/Uvicorn
```nginx
location / {
    proxy_pass http://127.0.0.1:8000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - Application Servers and Containers Tomcat HAProxy](./13-Application-Servers-and-Containers-Tomcat-HAProxy.md) | [README](./README.md) | [15 - Logging Formats Log Analysis Access and Error Logs](./15-Logging-Formats-Log-Analysis-Access-and-Error-Logs.md) |
