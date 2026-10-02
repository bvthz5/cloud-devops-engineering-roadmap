# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Nginx Diagnostic Decision Tree

```text
Problem: Nginx Fails to Start, Throws Errors, or Drops Requests
  │
  ├──► Does the configuration syntax pass?
  │      └── Run: nginx -t (Validates syntax and file permissions)
  │
  ├──► Is Nginx returning 502 Bad Gateway?
  │      ├── Upstream backend crashed or not listening on port.
  │      ├── SELinux blocking network proxying (setsebool -P httpd_can_network_connect 1).
  │      └── FastCGI / Uvicorn / Gunicorn socket permission denied.
  │
  ├──► Is Nginx returning 504 Gateway Timeout?
  │      ├── Upstream backend processing time > proxy_read_timeout (default 60s).
  │      └── Database lock or network firewall dropping upstream packets.
  │
  └──► Is Nginx returning 413 Request Entity Too Large?
         └── Body exceeds client_max_body_size (default 1m). Increase setting.
```

---

## 2. Zero-Downtime Hot Reload vs Restart

Never use `systemctl restart nginx` in production because it terminates active worker processes immediately, severing in-flight user downloads and WebSocket connections!

```bash
# Test syntax first
nginx -t

# Perform graceful zero-downtime hot reload
systemctl reload nginx
# OR send HUP signal directly to master process
kill -HUP $(cat /var/run/nginx.pid)
```

### What Happens Internally During `reload` (`HUP`):
1. Master process re-reads configuration files and validates syntax.
2. Master launches a new set of worker processes running the new configuration.
3. Master signals old worker processes to perform graceful shutdown (`QUIT` signal).
4. Old workers finish serving all existing active connections, then terminate.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
