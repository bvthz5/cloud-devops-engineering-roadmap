# 03 — Subnetting Fundamentals and Netmasks

Subnetting divides a large IP network into smaller, isolated, and logically distinct sub-networks.

---

## 1. Network Bits vs Host Bits

A 32-bit IP address is divided into two sections by the **Subnet Mask**:
```text
[ Network Portion (Prefix) ] [ Host Portion ]
```
- In the Subnet Mask, **1s represent Network bits**, and **0s represent Host bits**:
```text
Subnet Mask: 255.255.255.0
Binary:      11111111.11111111.11111111.00000000 (24 network bits, 8 host bits)
```

---

## 2. The Usable Host Formula

For any subnet with $H$ host bits:
$$	ext{Total Addresses} = 2^H$$
$$	ext{Usable Hosts} = 2^H - 2$$

### Why subtract 2?
1. **Network Address (All host bits = 0):** Identifies the network itself (e.g. `192.168.1.0`).
2. **Broadcast Address (All host bits = 1):** Broadcasts packets to every host on the subnet (e.g. `192.168.1.255`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Private vs Public IP Addresses RFC1918](./02-Private-vs-Public-IP-Addresses-RFC1918.md) | [Index](../../../README.md) | [04 - CIDR Classless Inter Domain Routing →](./04-CIDR-Classless-Inter-Domain-Routing.md) |
