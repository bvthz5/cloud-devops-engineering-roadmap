# 04 — HTTP Caching and Conditional Requests

Effective HTTP caching offloads 90%+ of traffic from database backends and significantly improves user latency.

---

## 1. Cache-Control Header Directives

- `max-age=3600`: Cache content for up to 3600 seconds (1 hour).
- `no-cache`: Must validate with the origin server before serving cached response (conditional request).
- `no-store`: **Never cache anywhere** (crucial for sensitive personal data, banking, auth tokens).
- `public`: Any intermediary (CDN, proxy) can cache.
- `private`: Only the end-user's browser can cache (not shared CDNs).
- `immutable`: Content will never change; never send validation requests (used for fingerprinted static assets like `app.4f3a2.js`).

---

## 2. Validation with ETags (`304 Not Modified`)

An **ETag** is a cryptographic hash of a resource representation:
1. Client requests file. Server returns: `ETag: "686897696a7c76"` and `HTTP 200 OK`.
2. When client requests file again, it sends: `If-None-Match: "686897696a7c76"`.
3. If file hasn't changed, server responds with **`HTTP 304 Not Modified`** with zero body payload, saving bandwidth!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - HTTPS & TLS Termination](./03-HTTPS-TLS-Termination-and-Certificates.md) | [README](./README.md) | [05 - WebSockets & gRPC](./05-WebSockets-SSE-and-gRPC-Protocols.md) |
