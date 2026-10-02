# 02 - BGP Path Selection Attributes and Metrics

## 1. The BGP Path Selection Algorithm

When a BGP router receives multiple routes for the exact same destination CIDR prefix, it breaks ties using this strict deterministic sequence:

```
Step 1: Highest WEIGHT (Cisco-proprietary, local to router only)
   ↓
Step 2: Highest LOCAL PREFERENCE (Propagated across entire local AS)
   ↓
Step 3: Prefer locally originated route (network/aggregate statement)
   ↓
Step 4: Shortest AS-PATH length (Fewest AS hops)
   ↓
Step 5: Lowest ORIGIN type (IGP < EGP < Incomplete)
   ↓
Step 6: Lowest MED (Multi-Exit Discriminator - metric advertised by neighbor)
   ↓
Step 7: Prefer eBGP over iBGP routes
   ↓
Step 8: Lowest IGP metric to BGP next hop
   ↓
Step 9: Lowest BGP Router ID (Final tie-breaker)
```

---

## 2. Engineering Outbound vs. Inbound Traffic

### Influencing Outbound Traffic: Local Preference
To prefer ISP-A over ISP-B for all traffic leaving your company, set `Local Preference = 200` on ISP-A and `Local Preference = 100` on ISP-B.

### Influencing Inbound Traffic: AS-Path Prepending
You cannot force the internet to choose your preferred link, but you can artificially lengthen your advertised path to discourage traffic on a backup link:
```
Primary Link Advertisement:   Prefix 203.0.113.0/24 -> AS-Path: [65001]
Backup Link Advertisement:    Prefix 203.0.113.0/24 -> AS-Path: [65001 65001 65001 65001]
```
Routers on the internet will prefer the primary link because its AS-Path length (1) is shorter than the backup link (4).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - BGP Architecture](./01-BGP-Fundamentals-Autonomous-Systems-and-eBGP-vs-iBGP.md) | [README](./README.md) | [03 - Cloud Interconnects](./03-Cloud-Dedicated-Interconnects-DirectConnect-and-ExpressRoute.md) |
