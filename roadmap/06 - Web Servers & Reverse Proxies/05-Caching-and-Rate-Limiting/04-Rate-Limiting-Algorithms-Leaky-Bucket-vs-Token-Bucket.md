# 04 - Rate Limiting Algorithms: Leaky Bucket vs Token Bucket

## 1. The Leaky Bucket Algorithm in Nginx

Nginx implements the **Leaky Bucket** algorithm via `ngx_http_limit_req_module`.

```text
                  Incoming Bursty Requests (e.g. 20 requests at once)
                                   │
                                   ▼
                    +─────────────────────────────+
                    |        Bucket (burst=20)    |
                    | (Holds temporary surges)    |
                    +──────────────┬──────────────+
                                   │
                                   ▼
              Smooth, Leaking Rate (e.g. rate=10r/s)
                 (1 request allowed every 100ms)
                                   │
                                   ▼
                    [ Backend Application Server ]
```

---

## 2. Nginx `limit_req_zone` and `burst` Mechanics

```nginx
http {
    # 1. Define Zone: Key on client IP ($binary_remote_addr consumes only 4 bytes in memory)
    # Zone size: 10MB holds ~160,000 unique IP states
    # Rate: 10 requests per second (1 request per 100ms)
    limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;

    # Customize rejection HTTP status code (Default is 503; 429 is RFC standard)
    limit_req_status 429;

    server {
        listen 80;

        location /api/ {
            # Burst: Allows surges up to 20 requests.
            # nodelay: Processes the burst requests IMMEDIATELY without artificially delaying them,
            # but marks bucket slots as consumed until they leak out over time!
            limit_req zone=api_limit burst=20 nodelay;

            proxy_pass http://api_backend;
        }
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Cache Invalidation & Stale Content](./03-Cache-Invalidation-Bypass-and-Stale-Revalidate.md) | [README](./README.md) | [05 - Connection Limiting & Bandwidth](./05-Connection-Limiting-and-Bandwidth-Throttling.md) |
