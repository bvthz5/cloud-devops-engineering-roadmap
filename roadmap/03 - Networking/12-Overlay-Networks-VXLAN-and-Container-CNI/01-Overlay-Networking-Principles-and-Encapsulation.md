# 01 - Overlay Networking Principles and Encapsulation

## 1. Underlay vs. Overlay Networks

- **Underlay Network:** The physical or cloud fabric network (cables, Top-of-Rack switches, routers, AWS VPC subnets) responsible for moving raw IP packets between physical or virtual hosts.
- **Overlay Network:** A virtual, software-defined network constructed on top of the underlay network. Packets from virtual machines or containers are encapsulated inside standard underlay network packets (e.g. UDP).

```
+-----------------------------------------------------------------------------------+
| OVERLAY NETWORK: Pod A (10.244.1.15) ─────────────► Pod B (10.244.2.88)            |
+-----------------------------------------------------------------------------------+
                                         │
                              Packet Encapsulation
                                         ▼
+-----------------------------------------------------------------------------------+
| UNDERLAY NETWORK: Node 1 (192.168.1.10) ──────────► Node 2 (192.168.1.20)         |
| (Physical switches only see packets traveling between Node 1 and Node 2)          |
+-----------------------------------------------------------------------------------+
```

---

## 2. Why Use Overlays in Containers?
1. **Decoupling from Physical Infrastructure:** Cloud providers limit the number of secondary IP addresses per VM (e.g. AWS ENI limits). An overlay allows running 1,000 Pods on a single node without allocating cloud VPC IPs.
2. **Multi-Tenancy Isolation:** Virtual Network Identifiers (VNIs) isolate tenant traffic completely.
3. **Layer 2 Adjacency across Layer 3:** Containers can communicate as if they were on the same local L2 Ethernet broadcast domain, even when physical nodes reside across different subnets.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - VXLAN & Geneve](./02-VXLAN-and-Geneve-Protocol-Deep-Dive.md) |
