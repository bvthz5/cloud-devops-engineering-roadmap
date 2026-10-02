# 03 - Cache-Control Headers and Invalidation Strategies

## 1. Modern Cache-Control Directives

```http
Cache-Control: public, max-age=60, s-maxage=3600, stale-while-revalidate=86400
```

| Directive | Target | Explanation |
|---|---|---|
| `max-age=60` | Browser Client | Browser caches the asset for 60 seconds before revalidating. |
| `s-maxage=3600` | Shared Proxy / CDN | Edge CDN caches the asset for 1 hour (3600s), ignoring `max-age`. |
| `stale-while-revalidate=86400` | Browser & CDN | If cached item is expired, serve the stale version instantly to the user while fetching a fresh copy in the background! (Zero user-perceived latency). |
| `stale-if-error=604800` | CDN Edge | If origin server is crashing (HTTP 500), serve the expired cache item for up to 7 days instead of showing an error page. |

---

## 2. Cache Invalidation Patterns

1. **Content Hashing (Best Practice):** Embed asset content hashes in URLs: `bundle.a8f2c1.js`. Cache indefinitely (`Cache-Control: public, max-age=31536000, immutable`). Never purge!
2. **Surrogate Keys / Cache Tags:** Origin tags responses: `Cache-Tag: product-1234, category-shoes`. When inventory changes, issue an instant API purge call to the CDN: `PURGE /tags product-1234`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - CDN Architecture PoPs and Edge Caching](./02-CDN-Architecture-PoPs-and-Edge-Caching.md) | [Index](../../../README.md) | [04 - Edge Compute Cloudflare Workers Lambda Edge Fastly Compute →](./04-Edge-Compute-Cloudflare-Workers-Lambda-Edge-Fastly-Compute.md) |
