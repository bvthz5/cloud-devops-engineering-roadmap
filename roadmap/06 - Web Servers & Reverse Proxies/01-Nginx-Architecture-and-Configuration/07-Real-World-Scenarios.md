# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Regex Location Precedence Bypass Outage

### Context & Incident
An enterprise e-commerce platform configured an authentication check on `/admin` routes. Users navigating to `/admin` were forwarded to an identity provider. One morning, security logs revealed that unauthenticated external attackers downloaded internal financial reports directly from `/admin/exports/2026-q1.csv`.

### Root Cause
The configuration contained:
```nginx
# Intended protection
location /admin/ {
    auth_request /auth;
    proxy_pass http://admin_backend;
}

# Static file acceleration rule defined elsewhere in config
location ~* \.(csv|xlsx|pdf|zip)$ {
    root /var/data/exports;
    expires 30d;
}
```
Because `/admin/` was a **standard prefix match**, regular expression matches take precedence! When a request for `/admin/exports/2026-q1.csv` arrived, Nginx selected the regex block, completely bypassing the `auth_request` security filter!

### Solution: Preferential Prefix Modifier (`^~`)
```nginx
# FIX: ^~ forces prefix match to stop regex evaluation!
location ^~ /admin/ {
    auth_request /auth;
    proxy_pass http://admin_backend;
}
```

---

## Scenario 2: Worker Starvation and File Descriptor Exhaustion

### Context & Incident
During Black Friday traffic, Nginx instances suddenly dropped 40% of incoming connections with HTTP 500/502. The error logs were flooded with:
`2026/10/02 09:12:00 [alert] 1234#1234: *54321 socket() failed (24: Too many open files)`

### Root Cause
Operating system user limits defaulted `nofile` to 1024, while `worker_connections` was set to 4096. When open client connections plus upstream proxy sockets exceeded 1024, the Linux kernel refused to allocate new socket descriptors.

### Solution: Systemd & Nginx Resource Limit Alignment
1. In `/etc/nginx/nginx.conf`:
   ```nginx
   worker_rlimit_nofile 65535;
   ```
2. In `/etc/systemd/system/nginx.service.d/override.conf`:
   ```ini
   [Service]
   LimitNOFILE=65535
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Nginx Performance Tuning](./06-Nginx-Performance-Tuning-and-Kernel-Directives.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
