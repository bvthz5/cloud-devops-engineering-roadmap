# 12 - CDN & Anycast: Quick Revision Cheat Sheet

## Essential Directives Cheat Sheet

| Directive | Intended Receiver | Meaning |
|---|---|---|
| `public` | Any cache | Cacheable by browser and CDN |
| `private` | Browser only | Forbidden from being cached by CDN/shared proxy |
| `no-cache` | Any cache | Must revalidate with origin (ETag) before serving |
| `no-store` | Any cache | Never write response to disk or cache memory |
| `s-maxage=N` | CDN Edge | Overrides `max-age` specifically for the CDN |
| `immutable` | Browser | Content will NEVER change during its lifetime |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (14-Modern-Web-Protocols-HTTP3-QUIC-and-gRPC) →](../14-Modern-Web-Protocols-HTTP3-QUIC-and-gRPC/01-Evolution-from-TCP-to-QUIC-UDP-Transport.md) |
