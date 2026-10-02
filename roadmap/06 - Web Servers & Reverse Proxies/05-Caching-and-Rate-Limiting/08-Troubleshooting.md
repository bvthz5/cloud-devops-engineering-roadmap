# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Inspecting Cache Performance via `$upstream_cache_status`

Nginx populates `$upstream_cache_status` with one of seven states:
- **`HIT`**: Response was served directly from cache.
- **`MISS`**: Response was not in cache; fetched from backend and cached.
- **`EXPIRED`**: Cache entry expired; refreshed from backend.
- **`UPDATING`**: Stale cache served while background worker fetches new copy.
- **`STALE`**: Stale cache served because backend returned error (502/503).
- **`BYPASS`**: Cache was skipped due to `proxy_cache_bypass`.

```bash
# Test with curl and inspect header
curl -I https://example.com/api/products
# Output:
# HTTP/1.1 200 OK
# X-Cache-Status: HIT
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
