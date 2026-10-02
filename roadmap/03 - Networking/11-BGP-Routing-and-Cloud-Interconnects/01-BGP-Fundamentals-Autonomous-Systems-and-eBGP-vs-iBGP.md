# 01 - BGP Fundamentals: Autonomous Systems and eBGP vs. iBGP

## 1. What is an Autonomous System (AS)?

An **Autonomous System (AS)** is a collection of connected IP routing prefixes under the control of a single administrative entity (e.g. AWS, Google, Microsoft, an ISP, or a large enterprise) that presents a common, clearly defined routing policy to the internet.

- **Public ASNs:** Assigned by IANA/RIRs (1 to 64495 for 16-bit, and up to 4,200,000,000+ for 32-bit ASNs). Globally visible.
- **Private ASNs:** Reserved for private enterprise and cloud use:
  - 16-bit: `64512` to `65534` (e.g., standard AWS VPC default ASN is `64512`).
  - 32-bit: `4200000000` to `4294967294`.

---

## 2. eBGP vs. iBGP

```
            [ Autonomous System 65001 ]                     [ Autonomous System 65002 ]
         +-------------------------------+               +-------------------------------+
         |                               |               |                               |
         |  [Router A] ◄─── iBGP ───► [Router B] ◄── eBGP ──► [Router C] ◄─── iBGP ───► [Router D]
         |                               |               |                               |
         +-------------------------------+               +-------------------------------+
```

| Dimension | External BGP (eBGP) | Internal BGP (iBGP) |
|---|---|---|
| **Peering Partners** | Routers in **different** Autonomous Systems | Routers within the **same** Autonomous System |
| **Default IP TTL** | `TTL = 1` (Must be directly connected physically or enable `ebgp-multihop`) | `TTL = 255` (Can span multiple internal hops) |
| **Loop Prevention** | **AS-Path Check:** If local ASN is seen in the AS-Path attribute, drop the route! | **Split-Horizon Rule:** Routes learned via iBGP are never advertised to another iBGP peer (requires full-mesh or Route Reflectors) |
| **Next-Hop Behavior** | Next-hop IP rewritten to local router interface IP | Next-hop IP unchanged from eBGP border router by default (`next-hop-self` required) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (10-Network-Troubleshooting-Tools)](../10-Network-Troubleshooting-Tools/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - BGP Path Selection Attributes and Metrics →](./02-BGP-Path-Selection-Attributes-and-Metrics.md) |
