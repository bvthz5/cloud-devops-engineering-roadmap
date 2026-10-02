# 05 - Connection Limiting and Bandwidth Throttling

## 1. Connection Limiting (`limit_conn_zone`)

While rate limiting limits requests per unit of time, **connection limiting** restricts the number of active, simultaneous TCP connections maintained by a single client IP:

```nginx
http {
    # Track simultaneous connections per IP
    limit_conn_zone $binary_remote_addr zone=addr_limit:10m;

    server {
        listen 80;

        location /downloads/ {
            # Max 2 concurrent open connections per client IP
            limit_conn addr_limit 2;

            # Bandwidth Throttling:
            # Send first 10MB at full speed, then throttle to 500 KB/sec
            limit_rate_after 10m;
            limit_rate 500k;

            root /var/data;
        }
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Rate Limiting Algorithms](./04-Rate-Limiting-Algorithms-Leaky-Bucket-vs-Token-Bucket.md) | [README](./README.md) | [06 - DDoS Mitigation & Security Throttling](./06-DDoS-Mitigation-and-IP-Reputation-Filtering.md) |
