# 05 — Layer 3 Network: IP, Routing, and Routers

Layer 3 enables internetworking: forwarding packets across multiple independent networks to their final destination across the globe.

---

## 1. IPv4 Packet Header Anatomy

A standard IPv4 header is 20 bytes long:

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |        Header Checksum        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source IP Address                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination IP Address                     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

## 2. Key Header Fields Explained

- **TTL (Time to Live):** Decremented by 1 at every router hop. When TTL reaches 0, the router discards the packet and returns an `ICMP Time Exceeded` error. **Prevents packets from looping forever during routing loops!**
- **Protocol:** Identifies Layer 4 payload (`6` = TCP, `17` = UDP, `1` = ICMP).
- **Flags & Fragment Offset:** Manages IP packet fragmentation when payload exceeds path MTU.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Layer 2 Data Link](./04-Layer-2-Data-Link-MAC-and-Switches.md) | [README](./README.md) | [06 - L4 vs L7 in Cloud](./06-Layer-4-vs-Layer-7-Networking-in-Cloud.md) |
