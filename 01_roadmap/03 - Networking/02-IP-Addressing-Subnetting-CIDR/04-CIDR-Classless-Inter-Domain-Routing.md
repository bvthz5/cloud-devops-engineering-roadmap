# 04 — CIDR: Classless Inter-Domain Routing

CIDR (RFC 1519) introduced prefix length notation (e.g. `/24`) to eliminate rigid class boundaries.

---

## 1. CIDR Reference Cheat Sheet

| CIDR Prefix | Subnet Mask | Total IPs | Usable Hosts (Standard) | Usable Hosts (AWS/Azure) |
| :--- | :--- | :---: | :---: | :---: |
| **/16** | `255.255.0.0` | 65,536 | 65,534 | 65,531 |
| **/20** | `255.255.240.0` | 4,096 | 4,094 | 4,091 |
| **/24** | `255.255.255.0` | 256 | 254 | 251 |
| **/25** | `255.255.255.128` | 128 | 126 | 123 |
| **/26** | `255.255.255.192` | 64 | 62 | 59 |
| **/27** | `255.255.255.224` | 32 | 30 | 27 |
| **/28** | `255.255.255.240` | 16 | 14 | 11 |
| **/29** | `255.255.255.248` | 8 | 6 | 3 |
| **/30** | `255.255.255.252` | 4 | 2 (Point-to-point links) | 0 (Unusable!) |
| **/32** | `255.255.255.255` | 1 | 1 (Single Host / Loopback) | 1 |

---

## 2. Dividing a `/24` into Four Equal Subnets

Suppose you have `10.0.1.0/24` (256 IPs) and need 4 subnets:
- $2^2 = 4$, so borrow **2 bits** from the host portion: $24 + 2 = \mathbf{/26}$.
- Each `/26` has 64 total IPs:
  1. Subnet 1: `10.0.1.0/26` (Range: `10.0.1.0` – `10.0.1.63`)
  2. Subnet 2: `10.0.1.64/26` (Range: `10.0.1.64` – `10.0.1.127`)
  3. Subnet 3: `10.0.1.128/26` (Range: `10.0.1.128` – `10.0.1.191`)
  4. Subnet 4: `10.0.1.192/26` (Range: `10.0.1.192` – `10.0.1.255`)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Subnetting Fundamentals and Netmasks](./03-Subnetting-Fundamentals-and-Netmasks.md) | [Index](../../../README.md) | [05 - Cloud VPC Subnet Design Best Practices →](./05-Cloud-VPC-Subnet-Design-Best-Practices.md) |
