# 02 - Cloud VPC Architecture and Subnet Topology

## 1. Multi-AZ Enterprise VPC Blueprint

A production-grade Virtual Private Cloud (VPC) spans at least 3 Availability Zones (AZs) for high availability:

```
VPC CIDR: 10.0.0.0/16
├── AZ A
│   ├── Public Subnet A (10.0.1.0/24) ──► Route: 0.0.0.0/0 -> Internet Gateway (IGW)
│   │   └── NAT Gateway A (Elastic IP: 54.1.2.3)
│   └── Private Subnet A (10.0.10.0/24) ──► Route: 0.0.0.0/0 -> NAT Gateway A
│       └── Database / App Workloads
│
├── AZ B
│   ├── Public Subnet B (10.0.2.0/24) ──► Route: 0.0.0.0/0 -> Internet Gateway (IGW)
│   │   └── NAT Gateway B (Elastic IP: 54.1.2.4)
│   └── Private Subnet B (10.0.20.0/24) ──► Route: 0.0.0.0/0 -> NAT Gateway B
│       └── Database / App Workloads
│
└── Internet Gateway (Attached to VPC root)
```

---

## 2. Public vs. Private Subnets

- **Public Subnet:** A subnet whose route table has an explicit route `0.0.0.0/0` pointing to an **Internet Gateway (IGW)**. Resources (ALBs, Bastion hosts) must have public IPs to receive direct internet traffic.
- **Private Subnet:** A subnet whose route table has `0.0.0.0/0` pointing to a **NAT Gateway** (or no default route at all for isolated databases). Resources have only private RFC 1918 IPs.

---

## 3. High Availability NAT Gateways

NAT Gateways are **AZ-specific** services:
- **Antipattern:** Pointing Private Subnets in AZ-A, AZ-B, and AZ-C to a single NAT Gateway in AZ-A. If AZ-A experiences an outage, all private internet egress across the entire cloud region fails!
- **Best Practice:** Deploy one NAT Gateway per Availability Zone.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - VPN Protocols IPsec WireGuard and OpenVPN](./01-VPN-Protocols-IPsec-WireGuard-and-OpenVPN.md) | [Index](../../../README.md) | [03 - VPC Peering Architecture and Limitations →](./03-VPC-Peering-Architecture-and-Limitations.md) |
