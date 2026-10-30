# 01 - HTTP Caching Architecture and Headers

## 1. HTTP `Cache-Control` Directives

```text
+-----------------------------------------------------------------------------------+
|                        Key Cache-Control Directives                               |
+-------------------+---------------------------------------------------------------+
| Directive         | Architectural Behavior                                        |
+-------------------+---------------------------------------------------------------+
| public            | Response may be cached by ANY cache (Browser, CDN, Proxy).    |
| private           | Response is personalized; cacheable ONLY by end-user browser. |
| no-cache          | Cache MAY store response, but MUST validate with origin (304) |
|                   | before serving!                                               |
| no-store          | DO NOT store under any circumstances (Sensitive/PII data).    |
| max-age=N         | Content is fresh for N seconds from creation.                 |
| s-maxage=N        | Overrides max-age specifically for shared proxies / CDNs.     |
+-------------------+---------------------------------------------------------------+
```

---

## 2. Conditional Requests: `ETag` and `Last-Modified`

When cached content expires, the client sends a conditional validation request:
- **`If-None-Match: "33a64df5"`**: Client sends the previously stored `ETag` hash.
- If the content has not changed, the server returns **`HTTP 304 Not Modified`** with **zero response body**, saving network bandwidth and compute!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-SSL-TLS-Certificates-and-HTTPS)](../04-SSL-TLS-Certificates-and-HTTPS/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Nginx Microcaching and FastCGI Proxy Cache →](./02-Nginx-Microcaching-and-FastCGI-Proxy-Cache.md) |
