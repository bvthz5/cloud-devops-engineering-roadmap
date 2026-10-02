# 01 - Docker Network Architecture and Drivers

## 1. Built-In Network Drivers Comparison

```text
+-----------------------------------------------------------------------------------+
|                        Docker Network Driver Matrix                               |
+-------------------+---------------------------------------------------------------+
| Driver            | Architectural Behavior & Use Case                             |
+-------------------+---------------------------------------------------------------+
| **bridge**        | Default. Private virtual Ethernet switch on host. NAT routing.|
| **host**          | Bypasses network isolation; container uses host's network     |
|                   | stack directly (Max throughput, zero NAT overhead).           |
| **none**          | Complete network isolation (Loopback only). Air-gapped tasks. |
| **overlay**       | Multi-host routing across Swarm / K8s nodes via VXLAN tunnels.|
| **macvlan**       | Assigns physical MAC and real LAN IP to container (Legacy).   |
+-------------------+---------------------------------------------------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Bridge Networking & Veth Pairs](./02-Bridge-Networking-and-Veth-Pairs.md) |
