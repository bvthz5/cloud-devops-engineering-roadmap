# 12 - Overlay Networks & CNI: Quick Revision Cheat Sheet

## MTU Overhead Reference Table

| Setup | Physical MTU | Encapsulation | Required Pod MTU |
|---|---|---|---|
| AWS VPC CNI | 1500 / 9001 | None (Direct VPC) | 1500 / 9001 |
| Calico / Flannel VXLAN | 1500 | 50 Bytes | **1450** |
| Calico + WireGuard | 1500 | ~60 Bytes | **1440** |
| Geneve (OVN) | 1500 | ~72 Bytes | **1428** |

## Fast CNI Selection Guide
- Need native VPC IPs & AWS Security Groups? $ightarrow$ **AWS VPC CNI**
- Need extreme performance & L7 DNS/HTTP observability? $ightarrow$ **Cilium**
- Need battle-tested BGP peering to datacenter switches? $ightarrow$ **Project Calico**

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (13-CDN-Edge-Networks-and-Anycast-Routing) →](../13-CDN-Edge-Networks-and-Anycast-Routing/01-Anycast-Routing-Architecture-and-BGP.md) |
