# 02 - Nginx Microcaching and Proxy Cache

## 1. The Power of Microcaching (1-Second Caching)

On dynamic, high-traffic websites (news, sports scoreboards, stock tickers), content changes frequently, so developers often disable caching entirely.

**Microcaching** caches dynamic backend responses for just **1 single second**:
- Under 10,000 requests per second, the backend database handles only **1 request per second**!
- 9,999 requests are served directly from Nginx RAM cache in sub-millisecond time.
- Data is never more than 1 second stale.

```text
Without Microcache:
[ 10,000 req/sec ] ──► [ Nginx ] ──► [ 10,000 queries/sec ] ──► [ Database Overload & Crash! ]

With 1-Second Microcache:
[ 10,000 req/sec ] ──► [ Nginx Cache (1s TTL) ] ──► [ 1 query/sec ] ──► [ Database at 1% CPU ]
```

---

## 2. Nginx `proxy_cache` Configuration

```nginx
http {
    # Define cache zone in memory + storage path on disk
    # keys_zone=my_cache:10m (10MB holds ~80,000 cache keys)
    proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m
                     max_size=5g inactive=60m use_temp_path=off;

    server {
        listen 80;

        location /api/dynamic/ {
            proxy_pass http://backend_pool;
            proxy_cache my_cache;

            # Microcache 200 responses for 1 second
            proxy_cache_valid 200 1s;
            proxy_cache_valid 404 10s;

            # Custom cache key
            proxy_cache_key "$scheme$request_method$host$request_uri";

            # Expose cache hit status in response header for debugging
            add_header X-Cache-Status $upstream_cache_status always;
        }
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - HTTP Caching Architecture](./01-HTTP-Caching-Architecture-and-Headers.md) | [README](./README.md) | [03 - Cache Invalidation & Stale Content](./03-Cache-Invalidation-Bypass-and-Stale-Revalidate.md) |
