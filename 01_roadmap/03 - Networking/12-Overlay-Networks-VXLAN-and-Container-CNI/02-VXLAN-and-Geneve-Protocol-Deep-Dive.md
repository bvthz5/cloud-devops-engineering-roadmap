# 02 - VXLAN and Geneve Protocol Deep Dive

## 1. VXLAN (Virtual Extensible LAN - RFC 7348)

VXLAN encapsulates Layer 2 Ethernet frames inside Layer 4 UDP datagrams:
- **UDP Destination Port:** `4789` (Standard IANA port).
- **VNI (VXLAN Network Identifier):** A 24-bit identifier supporting **16,777,216 logical networks** (compared to standard 802.1Q VLANs which max out at 4,096).
- **VTEP (VXLAN Tunnel Endpoint):** The entity (Linux kernel module, Open vSwitch, or physical switch) that encapsulates and decapsulates the packets.

```
+---------------------------------------------------------------------------------------+
| Outer Ethernet | Outer IP Header | Outer UDP Header | VXLAN Header | Inner Ethernet | Payload |
|    (14 B)      |     (20 B)      |     (8 B)        |    (8 B)     |    (14 B)      | (Data)  |
+---------------------------------------------------------------------------------------+
|<──────────────────────── 50 Bytes Encapsulation Overhead ───────────────────────────>|
```

---

## 2. Geneve (Generic Network Virtualization Encapsulation - RFC 8926)

Geneve is the modern successor to VXLAN and NVGRE, adopted by **Open Virtual Network (OVN)** and modern cloud SDN fabrics.
- **Why Geneve?** VXLAN headers are fixed at 8 bytes. Geneve introduces flexible **TLV (Type-Length-Value)** option fields, allowing software-defined networking controllers to embed telemetry, security group contexts, and tracing metadata directly into the packet header.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Overlay Networking Principles and Encapsulation](./01-Overlay-Networking-Principles-and-Encapsulation.md) | [Index](../../../README.md) | [03 - Container Network Interface CNI Architecture →](./03-Container-Network-Interface-CNI-Architecture.md) |
