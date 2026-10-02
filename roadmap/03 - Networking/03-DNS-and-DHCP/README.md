# Module 03: DNS and DHCP

The Domain Name System (DNS) and Dynamic Host Configuration Protocol (DHCP) are the twin engines of network bootstrap and service discovery. Understanding DNS records, resolution hierarchies, caching mechanics, split-horizon routing, and DHCP leases is crucial for maintaining microservices and cloud infrastructure.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Dissect the **hierarchical DNS namespace** (Root, TLD, Authoritative, Recursive).
- Master all major **DNS Record Types** (A, AAAA, CNAME, ALIAS, MX, TXT, PTR, SRV).
- Trace the step-by-step recursive resolution flow from browser to root nameservers.
- Explain **DHCP DORA** (Discover, Offer, Request, Acknowledge) network configuration.
- Implement **Split-Horizon DNS** and Private Hosted Zones in AWS Route 53.
- Understand **DNSSEC**, DNS cache poisoning, and DNS over HTTPS (DoH).
- Diagnose production DNS latency and failures using `dig +trace`.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [DNS Architecture & Hierarchical Tree](./01-DNS-Architecture-and-Hierarchical-Tree.md) | Root zone, TLDs, Authoritative vs Recursive resolvers, and FQDNs | ✅ Complete |
| 02 | [DNS Record Types Deep Dive](./02-DNS-Record-Types-Deep-Dive.md) | A, AAAA, CNAME, ALIAS, MX, TXT, PTR, SRV, CAA, and TTL mechanics | ✅ Complete |
| 03 | [Complete DNS Resolution Walkthrough](./03-The-Complete-DNS-Resolution-Walkthrough.md) | Iterative vs recursive lookups, caching tiers, and packet flow | ✅ Complete |
| 04 | [DHCP Protocol & DORA Process](./04-DHCP-Protocol-and-DORA-Process.md) | Discover, Offer, Request, Acknowledge, lease renewal, and options | ✅ Complete |
| 05 | [Split-Horizon DNS & Private Hosted Zones](./05-Split-Horizon-DNS-and-Private-Hosted-Zones.md) | Route 53 Private Zones, resolving internal services vs public web | ✅ Complete |
| 06 | [DNSSEC & Modern DNS Security](./06-DNSSEC-and-DNS-Security.md) | Cache poisoning, cryptographic signing (RRSIG/DNSKEY), DoH, and DoT | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Kubernetes CoreDNS crash loops; high TTL delaying disaster recovery | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Deep diagnostics with `dig +trace`, `resolvectl status`, and flushing | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical DNS interview questions for DevOps, SRE, and Cloud roles | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Trace root delegation with `dig +trace`; inspect SRV & TXT records | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Record types reference matrix, resolution flow, and dig command flags | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 02: IP & Subnetting](../02-IP-Addressing-Subnetting-CIDR/README.md) | [Networking Master Index](../README.md) | [01 - DNS Architecture](./01-DNS-Architecture-and-Hierarchical-Tree.md) |
