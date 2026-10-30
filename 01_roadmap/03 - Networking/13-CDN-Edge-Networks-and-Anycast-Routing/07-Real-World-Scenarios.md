# 07 - CDN & Anycast: Real-World Production Scenarios

## Scenario 1: The Fastly Global Configuration Edge Outage

### Incident Summary
On June 8, 2021, major websites worldwide (Amazon, Reddit, Twitch, CNN, GitHub, The UK Government) collapsed simultaneously, returning `Error 503 Service Unavailable`.

### Root Cause
An enterprise customer triggered an undocumented software defect by applying a valid configuration change containing a specific combination of conditions. This bug triggered an unhandled exception across Fastly's edge Varnish proxy fleet globally, causing 85% of their network to return 503s.

### SRE Lessons Learned
1. Edge CDN software updates must implement **progressive rollouts (canary deployments)** across edge datacenters rather than pushing globally in a single atomic commit.
2. Mission-critical platforms must evaluate multi-CDN routing strategies.

---

## Scenario 2: Personal Session Data Leaked by Aggressive Edge Caching

### Incident Summary
An e-commerce company added `s-maxage=3600` to a generic NGINX block. Users logging into the site began seeing random other customers' names, shopping carts, and masked credit cards!

### Root Cause
The backend application failed to strip the `Set-Cookie` header on responses that contained `s-maxage`. The CDN cached the personalized HTML response and served it to thousands of subsequent visitors.

### Fix
Explicitly declare `Cache-Control: private, no-store` on all authenticated endpoints.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Dynamic Content Acceleration and TCP Optimization](./06-Dynamic-Content-Acceleration-and-TCP-Optimization.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
