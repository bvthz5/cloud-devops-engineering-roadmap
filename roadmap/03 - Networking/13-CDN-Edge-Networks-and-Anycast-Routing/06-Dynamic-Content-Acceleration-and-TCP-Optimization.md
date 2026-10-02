# 06 - Dynamic Content Acceleration and TCP Optimization

## 1. Accelerating Uncacheable API Traffic

Even when an API payload is completely dynamic (e.g. `POST /checkout`), a CDN dramatically reduces response times:

```
WITHOUT CDN:
Client (Australia) ───[ TCP 3-Way Handshake: 300ms ]───► Origin (Virginia)
Client (Australia) ───[ TLS 1.3 Handshake:   300ms ]───► Origin (Virginia)
Client (Australia) ───[ HTTP POST & Reply:   300ms ]───► Origin (Virginia)
Total Time before first data received: 900 ms!

WITH CDN ACCELERATION:
Client (Sydney) ───[ TCP & TLS Handshake: 10ms ]───► Sydney Edge PoP
Sydney Edge PoP maintains a WARM, POOLED TCP connection over private fiber to Virginia!
Total Time: 10ms + 150ms = 160 ms! (Over 5x faster!)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - DDoS Mitigation](./05-DDoS-Mitigation-Rate-Limiting-and-WAF-at-the-Edge.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
