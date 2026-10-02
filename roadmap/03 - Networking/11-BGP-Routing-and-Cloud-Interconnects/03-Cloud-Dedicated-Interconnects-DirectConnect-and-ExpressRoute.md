# 03 - Cloud Dedicated Interconnects: Direct Connect and ExpressRoute

## 1. Dedicated Cloud Connectivity Overview

Rather than routing mission-critical corporate traffic over the unpredictable public internet via IPsec VPN, enterprises order physical cross-connect fiber links at colocation facilities (Equinix, Digital Realty):

- **AWS Direct Connect (DX):** 1 Gbps, 10 Gbps, or 100 Gbps dedicated optical fiber.
- **Azure ExpressRoute:** Layer 2/3 private circuit connecting directly to Microsoft Enterprise Edge (MSEE) routers.
- **Google Cloud Dedicated Interconnect:** 10G/100G circuits terminating on Google Edge PoPs.

---

## 2. Dynamic Routing Architecture

```
[ Corporate Data Center ]                              [ AWS Region us-east-1 ]
  └── On-Prem Router (ASN 65100)                        └── Direct Connect Gateway (ASN 64512)
           │                                                        │
           ▼                                                        ▼
    [ Physical Fiber Cross-Connect: 10 Gbps / 802.1Q VLAN 100 ]
           │
           └───► BGP Session (eBGP Peering):
                 • Local ASN: 65100, Remote ASN: 64512
                 • Advertises On-Prem CIDR: 192.168.0.0/16
                 • Receives VPC CIDR: 10.0.0.0/16
```

---

## 3. High Availability Blueprint

For critical production SLA (99.99% uptime):
- Minimum **2 dedicated connections** terminating at **2 distinct colocation data centers (locations)**.
- Terminating on **2 distinct customer edge routers**.
- Establishing separate BGP peering sessions over each circuit.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Path Selection](./02-BGP-Path-Selection-Attributes-and-Metrics.md) | [README](./README.md) | [04 - Kubernetes BGP](./04-BGP-Control-Plane-in-Kubernetes-Calico-and-MetalLB.md) |
