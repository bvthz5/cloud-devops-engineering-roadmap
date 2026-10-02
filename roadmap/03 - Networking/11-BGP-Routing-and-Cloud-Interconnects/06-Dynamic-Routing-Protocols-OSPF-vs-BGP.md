# 06 - Dynamic Routing Protocols: OSPF vs. BGP

## 1. Comparison Matrix

| Dimension | OSPF (Open Shortest Path First) | BGP (Border Gateway Protocol) |
|---|---|---|
| **Routing Type** | Interior Gateway Protocol (IGP) | Exterior Gateway Protocol (EGP) |
| **Algorithm** | Link-State (Dijkstra SPF) | Path-Vector (Policy-based) |
| **Convergence** | Sub-second (Extremely fast) | Moderate to Slow (Seconds to minutes) |
| **Scalability** | Thousands of routes (Datacenter / Campus) | 1,000,000+ global internet routes |
| **Metric** | Cost (Calculated from link bandwidth) | Multiple attributes (Weight, Local Pref, AS-Path) |
| **Transport** | Raw IP Protocol 89 | TCP Port 179 |

---

## 2. Modern Datacenter Design: BGP Everywhere
While legacy enterprises used OSPF internally and BGP at the border, modern hyperscale cloud datacenters (RFC 7938 - BGP in Large Data Centers) run **eBGP directly to every Top-of-Rack (ToR) switch and server** because BGP provides fine-grained traffic policy control and avoids OSPF flooding convergence storms.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - BGP Security](./05-BGP-Security-RPKI-Route-Hijacking-and-Flap-Damping.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
