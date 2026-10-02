# Module 02: IP Addressing, Subnetting, and CIDR

IP addressing and subnet calculation form the foundational blueprint for designing cloud Virtual Private Clouds (VPCs), hybrid network interconnects, and Kubernetes container cluster pod networks. Miscalculating subnets leads to IP exhaustion, routing overlaps, and expensive network redesigns.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Understand IPv4 structure (32-bit dotted-decimal) and the historical classful model.
- Identify **RFC 1918 private IP ranges** and cloud link-local addresses (`169.254.169.254`).
- Calculate subnets, network IDs, broadcast addresses, and usable hosts using bitwise logic.
- Master **CIDR (Classless Inter-Domain Routing)** prefix notation (`/16` to `/32`).
- Architect production cloud VPC subnets accounting for **reserved cloud IPs** (AWS/Azure).
- Understand **IPv6 architecture** (128-bit hexadecimal, SLAAC, dual-stack networks).
- Troubleshoot IP exhaustion and route table conflicts with `ipcalc` and `arping`.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [IPv4 Addressing & Classful Architecture](./01-IPv4-Addressing-and-Classful-Architecture.md) | 32-bit address structure, octets, and historical Class A/B/C/D/E | ✅ Complete |
| 02 | [Private vs Public IP Addresses (RFC 1918)](./02-Private-vs-Public-IP-Addresses-RFC1918.md) | RFC 1918 ranges, CGNAT (100.64.0.0/10), and cloud link-local metadata | ✅ Complete |
| 03 | [Subnetting Fundamentals & Netmasks](./03-Subnetting-Fundamentals-and-Netmasks.md) | Host bits vs network bits, subnet mask tables, and usable host math | ✅ Complete |
| 04 | [CIDR: Classless Inter-Domain Routing](./04-CIDR-Classless-Inter-Domain-Routing.md) | Slash notation, VLSM (Variable Length Subnet Masking), and supernetting | ✅ Complete |
| 05 | [Cloud VPC Subnet Design Best Practices](./05-Cloud-VPC-Subnet-Design-Best-Practices.md) | AWS/Azure 5 reserved IPs, non-overlapping multi-VPC CIDRs, EKS sizing | ✅ Complete |
| 06 | [IPv6 Architecture & Migration](./06-IPv6-Architecture-and-Migration.md) | 128-bit hex format, zero compression (`::`), SLAAC, and dual-stack | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Production EKS pod IP exhaustion; overlapping VPC peering outage | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Subnet verification with `ipcalc`, checking overlaps, fixing ARP conflicts | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical subnetting interview questions for Cloud & DevOps | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Subnetting calculation drills, `ipcalc` automation, IPv6 address setup | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Subnet prefix reference table, host formulas, and reserved IP matrix | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 01: OSI & TCP/IP](../01-OSI-and-TCPIP-Models/README.md) | [Networking Master Index](../README.md) | [01 - IPv4 Addressing](./01-IPv4-Addressing-and-Classful-Architecture.md) |
