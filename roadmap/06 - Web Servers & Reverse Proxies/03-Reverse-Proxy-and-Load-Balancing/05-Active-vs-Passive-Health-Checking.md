# 05 - Active vs Passive Health Checking

## 1. Passive (In-Band) vs Active (Out-of-Band) Health Checks

```text
Passive Health Checking (In-Band - Open-Source Nginx Default):
- Evaluates live client requests.
- If a client request to Backend A fails or times out, Nginx records a failure.
- Once 'max_fails' is reached within 'fail_timeout', the server is temporarily marked dead.
* DOWNSIDE: Real end-users experience the failed requests!

Active Health Checking (Out-of-Band - HAProxy / Nginx Plus / Traefik):
- A background probe sends dedicated synthetic health requests (e.g. GET /healthz)
  every N seconds.
- Unhealthy backends are removed from routing BEFORE real users ever hit them.
* ADVANTAGE: Zero user-facing failures during backend crashes.
```

---

## 2. Passive Health Check Configuration in Nginx

```nginx
upstream production_fleet {
    # If 3 requests fail within a 10s window, suspend server for 30s
    server 10.0.1.10:8080 max_fails=3 fail_timeout=30s;
    server 10.0.1.11:8080 max_fails=3 fail_timeout=30s;
}

server {
    listen 80;
    location / {
        proxy_pass http://production_fleet;
        
        # Transparently retry next upstream server if primary returns error or times out
        proxy_next_upstream error timeout invalid_header http_502 http_503 http_504;
        proxy_next_upstream_tries 3;
        proxy_next_upstream_timeout 5s;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Upstream Management & Buffer Tuning](./04-Upstream-Management-and-Buffer-Tuning.md) | [README](./README.md) | [06 - Header Manipulation & Client IP](./06-Header-Manipulation-and-Client-IP-Preservation.md) |
