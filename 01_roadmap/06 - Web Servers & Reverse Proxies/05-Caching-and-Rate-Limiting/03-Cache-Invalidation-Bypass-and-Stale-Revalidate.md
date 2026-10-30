# 03 - Cache Invalidation, Bypass, and Stale-While-Revalidate

## 1. Defeating the Cache Stampede (Thundering Herd)

When a popular cached item expires, hundreds of concurrent requests may miss the cache simultaneously, sending hundreds of duplicate queries to the backend database at once (**Cache Stampede**).

Nginx completely eliminates this via **`proxy_cache_use_stale updating`** and **`proxy_cache_lock`**:

```nginx
location / {
    proxy_pass http://backend_app;
    proxy_cache my_cache;
    proxy_cache_valid 200 10m;

    # 1. When cache expires, only 1 single request goes to backend to fetch fresh copy!
    proxy_cache_use_stale error timeout updating http_500 http_502 http_503;

    # 2. While backend is updating, all other concurrent requests receive stale cache instantly!
    proxy_cache_background_update on;

    # 3. Lock prevents parallel backend misses
    proxy_cache_lock on;
    proxy_cache_lock_timeout 5s;
}
```

---

## 2. Dynamic Cache Bypass for Authenticated Users

```nginx
# Do NOT cache if user has an auth session cookie or Authorization header
map $http_authorization $no_cache {
    default 0;
    "~.+"   1;
}

server {
    location / {
        proxy_pass http://backend;
        proxy_cache my_cache;

        proxy_no_cache $no_cache $cookie_session_id;
        proxy_cache_bypass $no_cache $cookie_session_id;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Nginx Microcaching and FastCGI Proxy Cache](./02-Nginx-Microcaching-and-FastCGI-Proxy-Cache.md) | [Index](../../../README.md) | [04 - Rate Limiting Algorithms Leaky Bucket vs Token Bucket →](./04-Rate-Limiting-Algorithms-Leaky-Bucket-vs-Token-Bucket.md) |
