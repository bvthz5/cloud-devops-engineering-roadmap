# 12 - Quick-Revision & Enterprise Cheat Sheet

```nginx
# Security Headers Golden Standard
add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "DENY" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Content-Security-Policy "default-src 'self';" always;
server_tokens off;

# Slowloris Defense Timeouts
client_header_timeout 10s;
client_body_timeout 10s;
keepalive_timeout 30s 15s;
send_timeout 10s;
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [01 - Nginx Architecture](../01-Nginx-Architecture-and-Configuration/README.md) |
