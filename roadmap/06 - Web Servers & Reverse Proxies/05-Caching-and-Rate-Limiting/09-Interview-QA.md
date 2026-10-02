# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the exact difference between `proxy_cache_bypass` and `proxy_no_cache`?
**Answer**: `proxy_cache_bypass` tells Nginx whether to **read** from the cache for the current request (if true, Nginx fetches from the backend instead of serving cached data). `proxy_no_cache` tells Nginx whether to **save** the backend response into the cache (if true, the response is never written to cache).

### Q2: What is a Cache Stampede (Thundering Herd) and how does Nginx mitigate it?
**Answer**: A cache stampede occurs when a high-traffic cached object expires, causing hundreds of concurrent requests to miss the cache simultaneously and overwhelm the origin database. Nginx mitigates this using `proxy_cache_use_stale updating`, which allows one request to revalidate with the origin while serving the existing stale cache entry to all other concurrent clients.

### Q3: How does the `nodelay` flag affect Nginx rate limiting?
**Answer**: By default, when a burst arrives, Nginx delays requests in the queue, releasing them at the steady leaky rate (e.g., 1 every 100ms). With `nodelay`, Nginx processes all requests within the burst allowance **immediately** without artificial delay, while marking the bucket capacity as occupied until it drains over time.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
