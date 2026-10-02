# 12 - Quick-Revision & Enterprise Cheat Sheet

```nginx
# 1. Rate Limiting Boilerplate (10 req/sec with burst of 20)
limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;
limit_req_status 429;

location /api/ {
    limit_req zone=api_limit burst=20 nodelay;
    proxy_pass http://backend;
}

# 2. Microcache Boilerplate (1s TTL + Stale-While-Revalidate)
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=cache_zone:10m max_size=1g;

location / {
    proxy_cache cache_zone;
    proxy_cache_valid 200 1s;
    proxy_cache_use_stale updating error timeout;
    proxy_cache_background_update on;
    add_header X-Cache-Status $upstream_cache_status always;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (06-HAProxy-High-Performance-Load-Balancing) →](../06-HAProxy-High-Performance-Load-Balancing/01-HAProxy-Architecture-and-Event-Driven-Engine.md) |
