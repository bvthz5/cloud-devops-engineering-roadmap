# 06 - Overlay Networks and Multi-Host VXLAN

## 1. VXLAN Encapsulation

Overlay networks span multiple physical Docker or Kubernetes nodes:
1. Container on Node 1 sends packet to `10.0.0.5` (located on Node 2).
2. The Docker overlay driver encapsulates the original Ethernet frame into a **UDP packet (Port 4789)**.
3. Node 1 sends the UDP packet across the physical underlay network to Node 2.
4. Node 2 decapsulates the UDP packet and delivers the raw frame to Container 2!

```text
[ Container 1 (10.0.0.4) ] ──► [ VXLAN Encapsulator (UDP:4789) ] ──► Physical Underlay Network
                                                                              │
[ Container 2 (10.0.0.5) ] ◄── [ VXLAN Decapsulator ] ◄───────────────────────┘
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Host None and Macvlan Networking](./05-Host-None-and-Macvlan-Networking.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
