# 17. Web Server Security Hardening Best Practices

## 1. Disable Server Information Disclosure
Prevent attackers from discovering server software versions:

```nginx
# Nginx config
http {
    server_tokens off;
}
```

```apache
# Apache config
ServerTokens Prod
ServerSignature Off
```

## 2. Restrict Request Body Sizes
Prevents buffer overflow and memory exhaustion attacks:

```nginx
client_max_body_size 10M;
client_body_buffer_size 128k;
```

## 3. Rate Limiting (DDoS & Brute Force Prevention)

```nginx
# Define rate limiting zone in http context (10 requests per second per IP)
limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;

server {
    location /api/login {
        limit_req zone=api_limit burst=5 nodelay;
        proxy_pass http://127.0.0.1:3000;
    }
}
```

## 4. Web Application Firewall (ModSecurity WAF)
Integrate ModSecurity with OWASP Core Rule Set (CRS) to block SQL Injection (SQLi), Cross-Site Scripting (XSS), and Remote Code Execution (RCE) attacks at the reverse proxy layer.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - HTTP Status Codes and Troubleshooting 502 504 403](./16-HTTP-Status-Codes-and-Troubleshooting-502-504-403.md) | [README](./README.md) | [18 - Hands On Lab Ubuntu Nginx Reverse Proxy Setup](./18-Hands-On-Lab-Ubuntu-Nginx-Reverse-Proxy-Setup.md) |
