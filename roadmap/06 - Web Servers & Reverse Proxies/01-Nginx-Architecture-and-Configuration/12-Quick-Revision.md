# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Validate Configuration
nginx -t

# Hot Reload without Downtime
nginx -s reload
# or
systemctl reload nginx

# Check Version & Compiled Modules
nginx -V
```

```nginx
# Location Matching Priority Summary:
# 1. =  (Exact Match)
# 2. ^~ (Preferential Prefix - skips regex)
# 3. ~  (Case-Sensitive Regex) & ~* (Case-Insensitive Regex) in order of file appearance
# 4. /  (Standard Prefix - longest wins)

# Zero-Copy Optimization Directives
sendfile on;
tcp_nopush on;
tcp_nodelay on;

# Security Header Baseline
add_header X-Content-Type-Options nosniff always;
add_header X-Frame-Options SAMEORIGIN always;
add_header X-XSS-Protection "1; mode=block" always;
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [02 - Apache HTTP Server](../02-Apache-HTTP-Server/README.md) |
