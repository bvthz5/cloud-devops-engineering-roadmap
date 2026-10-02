# 01 — DNS Architecture and Hierarchical Tree

The Domain Name System (DNS) is a globally distributed, hierarchical database that translates human-friendly domain names (`api.company.com`) into computer-routable IP addresses (`93.184.216.34`).

---

## 1. The Inverted DNS Tree Structure

```text
                           [ Root Zone: "." ]
               (13 Root Server clusters: a.root-servers.net to m)
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         ▼                         ▼                         ▼
   [ .com TLD ]              [ .org TLD ]              [ .io TLD ]
         │
         ▼
[ company.com Authoritative ]
         │
    ┌────┴────┐
    ▼         ▼
  [ api ]   [ www ]
```

---

## 2. Key Actors in DNS

1. **Root Nameservers:** 13 logical root server IP addresses (replicated via BGP Anycast across thousands of physical servers worldwide). They direct queries to Top-Level Domain (TLD) servers.
2. **TLD Nameservers:** Manage top-level domains like `.com`, `.net`, `.org`, and country-code TLDs (`.uk`, `.de`). They direct queries to authoritative nameservers.
3. **Authoritative Nameservers:** Hold the actual DNS records for a domain (e.g. AWS Route 53, Cloudflare).
4. **Recursive Resolvers:** Servers (e.g., Google `8.8.8.8`, Cloudflare `1.1.1.1`, ISP DNS) that do the legwork of querying root, TLD, and authoritative servers on behalf of client devices.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (02-IP-Addressing-Subnetting-CIDR)](../02-IP-Addressing-Subnetting-CIDR/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - DNS Record Types Deep Dive →](./02-DNS-Record-Types-Deep-Dive.md) |
