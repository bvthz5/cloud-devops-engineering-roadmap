# 02 - Internet Gateways (IGW), NAT Gateways & Egress-Only Gateways

> **Focus:** Internet connectivity, horizontal auto-scaling of Internet Gateways, NAT Gateway deployment patterns, Elastic IPs, NAT Gateway multi-AZ resiliency, and IPv6 Egress-Only IGWs.

---

## 🚪 1. Internet Gateway (IGW) vs. NAT Gateway

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│        Internet Gateway (IGW)        │       │             NAT Gateway              │
│                                      │       │                                      │
│  • Highly available, horizontally    │       │  • Deployed inside a PUBLIC subnet   │
│    scaled, redundant VPC component   │       │  • Requires an Elastic IP (EIP)      │
│  • Performs 1:1 NAT for public IPs   │       │  • Translates private IP to public   │
│  • Bidirectional: Inbound & Outbound │       │  • ONE-WAY: Outbound traffic only    │
│  • Zero bandwidth bottlenecks ($0)   │       │  • Billed per hour + per GB processed│
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

---

## 🛡️ 2. High Availability NAT Gateway Architecture

A NAT Gateway is a **zonal service**. If the Availability Zone where the NAT Gateway is deployed experiences an outage, private subnets routing through it lose internet connectivity!

```text
Production Multi-AZ Resilient Architecture:
  • AZ us-east-1a: Public Subnet A has NAT Gateway A ➔ Private Subnet A routes through NAT Gateway A.
  • AZ us-east-1b: Public Subnet B has NAT Gateway B ➔ Private Subnet B routes through NAT Gateway B.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - VPC Architecture CIDR Blocks and Subnetting](./01-VPC-Architecture-CIDR-Blocks-and-Subnetting.md) | [Index](../../../README.md) | [03 - Route Tables Security Groups and Network ACLs NACLs →](./03-Route-Tables-Security-Groups-and-Network-ACLs-NACLs.md) |
