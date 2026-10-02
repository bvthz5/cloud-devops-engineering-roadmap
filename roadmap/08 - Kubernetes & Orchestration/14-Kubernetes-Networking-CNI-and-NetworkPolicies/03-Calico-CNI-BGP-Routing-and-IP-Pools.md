# 03 - Calico CNI: BGP Routing and IP Pools

## 1. Calico Architecture

**Calico** provides high-scale enterprise networking and security policies using pure Layer 3 routing:
- **`Felix`:** The agent running on every node that programs Linux kernel routes and iptables rules.
- **`BIRD (BGP Client)`:** Distributes Pod IP routing information across worker nodes and physical Top-of-Rack (ToR) switches using the **Border Gateway Protocol (BGP)**.
- **Zero Encapsulation Overhead:** In flat networks, packets travel between nodes without VXLAN or Geneve encapsulation headers!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - CNI Specification](./02-Container-Network-Interface-CNI-Specification.md) | [README](./README.md) | [04 - Cilium & eBPF](./04-Cilium-CNI-eBPF-Datapath-and-High-Performance-Networking.md) |
