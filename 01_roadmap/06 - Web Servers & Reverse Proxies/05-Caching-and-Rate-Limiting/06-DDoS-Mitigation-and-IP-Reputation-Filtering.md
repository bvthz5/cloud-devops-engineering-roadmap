# 06 - DDoS Mitigation and Security Throttling

## 1. Multi-Tiered Defense Architecture

```text
Layer 1: Connection Limiting (limit_conn) ──► Rejects slowloris connection-holding attacks
Layer 2: Request Rate Limiting (limit_req) ──► Rejects HTTP request floods (429 Too Many Requests)
Layer 3: Whitelisting Trusted Networks    ──► Bypasses corporate offices and internal microservices
Layer 4: Aggressive Timeouts               ──► Drops idle hanging client sockets
```

---

## 2. Whitelisting Internal Subnets from Rate Limiting

```nginx
http {
    # Map client IP to determine if rate limiting applies
    geo $rate_limited_ip {
        default          $binary_remote_addr; # External clients: limited
        127.0.0.1        "";                  # Localhost: exempt
        10.0.0.0/8       "";                  # Internal VPC: exempt
        192.168.1.0/24   "";                  # Corporate Office: exempt
    }

    # If $rate_limited_ip evaluates to empty string (""), Nginx skips rate limiting!
    limit_req_zone $rate_limited_ip zone=general_api:10m rate=20r/s;

    server {
        location / {
            limit_req zone=general_api burst=50 nodelay;
            proxy_pass http://backend;
        }
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Connection Limiting and Bandwidth Throttling](./05-Connection-Limiting-and-Bandwidth-Throttling.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
