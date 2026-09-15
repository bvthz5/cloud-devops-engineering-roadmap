# 16. HTTP Status Codes & Troubleshooting Guide

## HTTP Status Code Taxonomy

- **2xx Success:** `200 OK`, `201 Created`, `204 No Content`.
- **3xx Redirection:** `301 Moved Permanently`, `302 Found`, `304 Not Modified`.
- **4xx Client Error:** `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests`.
- **5xx Server Error:** `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout`.

---

## Production Failure Scenarios & Diagnostic Matrix

### 1. HTTP 502 Bad Gateway
- **Symptom:** Nginx returns 502 HTML error page to client.
- **Root Cause:** Backend application server process (Node.js, Gunicorn, PHP-FPM) is crashed, not listening on target port/socket, or refused connection.
- **Diagnostic Commands:**
  ```bash
  sudo tail -n 20 /var/log/nginx/error.log
  # Error: connect() failed (111: Connection refused) while connecting to upstream
  
  # Check if backend app service is active
  systemctl status my_app
  sudo netstat -tulpn | grep :3000
  ```

### 2. HTTP 504 Gateway Timeout
- **Symptom:** Nginx waits 60s and returns 504 Gateway Timeout.
- **Root Cause:** Backend app is alive but taking longer than `proxy_read_timeout` to process database queries or external API calls.
- **Fix:** Increase timeouts or optimize backend DB query performance:
  ```nginx
  proxy_read_timeout 300s;
  proxy_connect_timeout 300s;
  ```

### 3. HTTP 403 Forbidden
- **Symptom:** Nginx rejects client with 403 error.
- **Root Cause:** Incorrect file system permissions on `root` path or SELinux/AppArmor blocking read access.
- **Fix:**
  ```bash
  sudo chown -R www-data:www-data /var/www/html
  sudo chmod -R 755 /var/www/html
  ```
