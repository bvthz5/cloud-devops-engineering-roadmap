# 02 - CDN Architecture, PoPs, and Edge Caching

## 1. CDN Topology: Edge to Origin

```
[ User Browser ]
       │  (1-5 ms RTT to local Edge PoP)
       ▼
[ CDN Edge PoP (L1 Cache) ] ──── Miss ────► [ Origin Shield (L2 Cache) ] ──── Miss ────► [ Origin VPC / ALB ]
       │ (Cached Content)                               │ (Aggregated Requests)
       ▼                                                ▼
 Return to User                                  Cache & Return
```

---

## 2. The Power of an Origin Shield
When 100,000 users across 50 global edge PoPs request a breaking news article simultaneously:
- **Without Origin Shield:** 50 edge PoPs simultaneously hit the central origin database (Cache Stampede).
- **With Origin Shield:** A centralized regional cache aggregates the requests. The origin server receives **exactly 1 request**, and the shield distributes it to all 50 edge PoPs.

---

## 3. Cache Hit Ratio (CHR)
$$\text{CHR} = \frac{\text{Cache Hits}}{\text{Cache Hits} + \text{Cache Misses}} \times 100$$
Targeting a CHR of $>95\%$ for static assets and $>75\%$ for cacheable API payloads drastically lowers cloud origin egress costs and compute sizing requirements.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Anycast Architecture](./01-Anycast-Routing-Architecture-and-BGP.md) | [README](./README.md) | [03 - Cache-Control](./03-Cache-Control-Headers-and-Invalidation-Strategies.md) |
