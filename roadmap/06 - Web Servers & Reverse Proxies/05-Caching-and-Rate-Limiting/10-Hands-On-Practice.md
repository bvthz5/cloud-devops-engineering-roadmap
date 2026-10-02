# 10 - Hands-On Practice Labs

## Lab 1: Production Microcaching with Stale-While-Revalidate

### Objective
Configure Nginx to cache dynamic API responses for 2 seconds with background stale revalidation and cache status header injection.

### Implementation
```nginx
proxy_cache_path /var/cache/nginx/api levels=1:2 keys_zone=api_cache:10m max_size=1g inactive=10m;

server {
    listen 80;

    location /api/v1/scores {
        proxy_pass http://scores_backend;
        proxy_cache api_cache;
        proxy_cache_valid 200 2s;

        proxy_cache_use_stale error timeout updating http_502 http_503;
        proxy_cache_background_update on;
        proxy_cache_lock on;

        add_header X-Cache-Status $upstream_cache_status always;
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
