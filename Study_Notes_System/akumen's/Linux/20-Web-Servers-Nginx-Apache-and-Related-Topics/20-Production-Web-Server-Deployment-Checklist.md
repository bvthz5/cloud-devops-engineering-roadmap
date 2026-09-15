# 20. Production Web Server Deployment Checklist

## Pre-Flight Readiness Checklist

- [ ] **Architecture & Syntax Validation:**
  - Ran `nginx -t` or `apache2ctl configtest` without syntax errors.
  - Set `worker_processes auto;` and tuned `worker_connections 2048+`.
- [ ] **TLS / Security Hardening:**
  - Enforced HTTPS-only redirection (301 redirect from Port 80 to 443).
  - Disabled weak TLS versions (TLSv1.0 & TLSv1.1 disabled; TLSv1.2 & TLSv1.3 enabled).
  - Configured Certbot auto-renewal timers (`certbot renew --dry-run`).
  - Added HSTS, `X-Frame-Options`, `X-Content-Type-Options` security headers.
  - Disabled `server_tokens` (hides software version).
- [ ] **Performance & Optimization:**
  - Enabled `sendfile on;` and `tcp_nopush on;`.
  - Configured Gzip / Brotli compression for text, CSS, JS, JSON.
  - Set up `proxy_cache` zones for static assets & static API endpoints.
  - Configured rate limiting zones (`limit_req_zone`) for sensitive endpoints.
- [ ] **Logging & Monitoring:**
  - Configured structured JSON access logging.
  - Verified log rotation (`/etc/logrotate.d/nginx`).
  - Integrated metric exporters (e.g., Prometheus Nginx Exporter).
