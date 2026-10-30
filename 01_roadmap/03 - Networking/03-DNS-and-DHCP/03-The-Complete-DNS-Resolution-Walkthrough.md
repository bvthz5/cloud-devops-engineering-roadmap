# 03 — The Complete DNS Resolution Walkthrough

When you type `https://api.example.com` into your browser, what exact sequence of network queries takes place?

---

## 1. The Resolution Flowchart

```text
[ 1. Local Cache Check ]
Browser Cache ──► OS DNS Cache (systemd-resolved) ──► /etc/hosts
       │ (Cache Miss)
       ▼
[ 2. Client Queries Recursive Resolver ]
Client (Laptop) ──( Recursive Query )──► Recursive DNS Resolver (8.8.8.8)
                                                    │
       ┌────────────────────────────────────────────┴────────────────────────────────────────────┐
       ▼ (Iterative Query 1)                                                                     ▼
[ Root Nameserver (".") ]                                                           (Checks Cache First)
       │ "I don't know api.example.com, but here are the .com TLD nameservers!"
       ▼
[ .com TLD Nameserver ]
       │ "I don't know api.example.com, but here are the authoritative nameservers for example.com!"
       ▼
[ example.com Authoritative Nameserver ] (Route 53 / Cloudflare)
       │ "api.example.com has IP 93.184.216.34 with TTL 300 seconds!"
       ▼
Recursive Resolver caches record for 300s and delivers IP to Client.
```

---

## 2. Recursive vs Iterative Queries

- **Recursive Query:** The client asks the resolver: *"Find the answer for me and return only the final IP."*
- **Iterative Query:** The resolver asks upstream servers: *"Give me the best answer you know right now, or refer me to the next nameserver in the hierarchy."*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - DNS Record Types Deep Dive](./02-DNS-Record-Types-Deep-Dive.md) | [Index](../../../README.md) | [04 - DHCP Protocol and DORA Process →](./04-DHCP-Protocol-and-DORA-Process.md) |
