# 05 - Caching and Rate Limiting

Caching and Rate Limiting are the two most powerful tools available to Platform Engineers to protect backend infrastructure from catastrophic collapse. Caching offloads heavy computational and database workloads, while rate limiting prevents API abuse, brute-force attacks, and DDoS starvation.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [HTTP Caching Architecture & Headers](./01-HTTP-Caching-Architecture-and-Headers.md) | `Cache-Control`, `max-age`, `s-maxage`, `no-cache` vs `no-store`, `ETag`, conditional validation. |
| 02 | [Nginx Microcaching & Proxy Cache](./02-Nginx-Microcaching-and-FastCGI-Proxy-Cache.md) | `proxy_cache_path`, memory zones, cache key calculation, 1-second dynamic microcaching. |
| 03 | [Cache Invalidation & Stale Content](./03-Cache-Invalidation-Bypass-and-Stale-Revalidate.md) | `proxy_cache_use_stale updating`, background revalidation, bypass headers, cache stampede defense. |
| 04 | [Rate Limiting: Leaky Bucket vs Token Bucket](./04-Rate-Limiting-Algorithms-Leaky-Bucket-vs-Token-Bucket.md) | Leaky bucket algorithm, `limit_req_zone`, `rate=10r/s`, `burst=20`, `nodelay` mechanics. |
| 05 | [Connection Limiting & Bandwidth Throttling](./05-Connection-Limiting-and-Bandwidth-Throttling.md) | `limit_conn_zone`, restricting concurrent connections per IP, `limit_rate` bandwidth throttling. |
| 06 | [DDoS Mitigation & Security Throttling](./06-DDoS-Mitigation-and-IP-Reputation-Filtering.md) | Returning HTTP 429 vs 503, whitelisting corporate gateways, geo-blocking, defense in depth. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Cache stampede thundering herd, private user data cache poisoning. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Inspecting `X-Cache-Status` (`HIT`, `MISS`, `BYPASS`), debugging rate limits, load benchmarking. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps questions on caching trade-offs, stale-while-revalidate, and bucket limits. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Microcaching dynamic API; Lab 2: Multi-tiered rate limiter; Lab 3: Cache status tracing. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for proxy_cache directives, limit_req syntax, and cache headers. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - SSL/TLS Certificates & HTTPS](../04-SSL-TLS-Certificates-and-HTTPS/README.md) | [README](./README.md) | [01 - HTTP Caching Architecture](./01-HTTP-Caching-Architecture-and-Headers.md) |
