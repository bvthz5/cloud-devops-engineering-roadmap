# 04 — Layer 2 Data Link: MAC, Ethernet, and Switches

Layer 2 is responsible for moving data across a single physical local network segment (LAN).

---

## 1. The MAC (Media Access Control) Address

A **MAC address** is a 48-bit (6-byte) hardware identifier burned into network interface cards (NICs).
Format: `00:1A:2B:3C:4D:5E`
- **First 24 bits (OUI):** Organizationally Unique Identifier (assigned to hardware manufacturer, e.g., Cisco, Intel, VMware).
- **Last 24 bits:** Unique device serial number.

---

## 2. Ethernet Frame Anatomy

```text
+----------------+----------------+-----------+--------------------+---------------+
| Dest MAC (6B)  | Source MAC (6B)| Type (2B) | Payload (46-1500B) | FCS / CRC (4B)|
+----------------+----------------+-----------+--------------------+---------------+
```
- **Type (EtherType):** Identifies the Layer 3 protocol payload (`0x0800` for IPv4, `0x86DD` for IPv6, `0x0806` for ARP).
- **Payload:** Maximum Transmission Unit (**MTU = 1500 bytes** standard).
- **FCS (Frame Check Sequence):** 32-bit CRC checksum to detect transmission bit corruption. Corrupted frames are silently dropped by hardware!

---

## 3. How Switches Work: The CAM Table

A Layer 2 Network Switch maintains a **Content Addressable Memory (CAM) Table** mapping MAC addresses to physical switch ports:
1. When a frame arrives on Port 1 from MAC `A`, the switch records: `MAC A -> Port 1`.
2. If destination MAC `B` is already in the CAM table for Port 3, the switch forwards the frame directly to Port 3 (**Unicast**).
3. If destination MAC is unknown, the switch floods the frame out of all ports (**Unknown Unicast Flooding**).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Encapsulation and Decapsulation Data Flow](./03-Encapsulation-and-Decapsulation-Data-Flow.md) | [Index](../../../README.md) | [05 - Layer 3 Network IP Routing and Routers →](./05-Layer-3-Network-IP-Routing-and-Routers.md) |
