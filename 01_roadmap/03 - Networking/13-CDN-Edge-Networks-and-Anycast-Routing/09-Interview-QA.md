# 09 - CDN & Anycast: Interview Questions & Answers

### Q1: What is BGP Anycast and why is it used for CDNs and DNS?
**Answer:** Anycast allows multiple geographically separated edge servers to announce the exact same IP address via BGP. Routers on the internet automatically send packets to the topologically closest edge server based on standard BGP path metrics. It delivers ultra-low latency and distributes DDoS attacks across all global edge PoPs.

### Q2: What is the difference between `max-age` and `s-maxage` in HTTP Cache-Control?
**Answer:** `max-age` directs browser client caches how long to store the response. `s-maxage` (shared max-age) specifically instructs intermediate shared caches (such as CDNs and reverse proxies) how long to store the response, overriding `max-age` for the CDN while leaving browser caching independent.

### Q3: What is the purpose of `stale-while-revalidate`?
**Answer:** It instructs the CDN or browser to immediately return an expired (stale) cached asset to the client with zero latency, while asynchronously fetching an updated version from the origin in the background.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
