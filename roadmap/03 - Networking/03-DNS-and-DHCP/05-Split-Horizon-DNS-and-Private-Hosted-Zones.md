# 05 — Split-Horizon DNS and Private Hosted Zones

Split-Horizon DNS provides different DNS responses to queries based on the network origin of the requesting client.

---

## 1. Architecture

```text
Public Internet Client ──► Queries "api.company.com" ──► Resolves to Public ALB IP: 203.0.113.50
                                                                   ▲
                                                                   │ (Split Horizon)
                                                                   ▼
Internal Cloud VPC Host ──► Queries "api.company.com" ──► Resolves to Private VPC IP: 10.0.1.25
```

- **Benefits:** Internal microservices communicate over private cloud IP interconnects with zero internet data transfer costs and enhanced security, while external clients connect via public load balancers.
- **Implementation:** AWS Route 53 Private Hosted Zones attached strictly to VPC IDs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - DHCP Protocol and DORA Process](./04-DHCP-Protocol-and-DORA-Process.md) | [Index](../../../README.md) | [06 - DNSSEC and DNS Security →](./06-DNSSEC-and-DNS-Security.md) |
