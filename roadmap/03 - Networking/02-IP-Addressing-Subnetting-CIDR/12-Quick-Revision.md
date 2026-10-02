# 12 — Quick Revision Cheat Sheet: Subnetting & CIDR

---

## 1. Quick Reference Formulas

- Total IPs in `/$P$`: $2^{32 - P}$
- Standard Usable Hosts: $2^{32 - P} - 2$
- Cloud (AWS/Azure) Usable Hosts: $2^{32 - P} - 5$

## 2. RFC 1918 Private Ranges

- `10.0.0.0/8` (10.0.0.0 – 10.255.255.255)
- `172.16.0.0/12` (172.16.0.0 – 172.31.255.255)
- `192.168.0.0/16` (192.168.0.0 – 192.168.255.255)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple Choice Questions](./11-MCQ.md) | [README](./README.md) | [Next Module: 03 - DNS & DHCP](../03-DNS-and-DHCP/README.md) |
