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
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [14 - Modern Web Protocols](../14-Modern-Web-Protocols-HTTP3-QUIC-and-gRPC/README.md) |
