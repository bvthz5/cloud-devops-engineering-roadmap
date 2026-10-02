# 05 — Cloud VPC Subnet Design Best Practices

Designing IP architecture in cloud providers (AWS, Azure, Google Cloud) requires understanding cloud-specific networking constraints.

---

## 1. The 5 Reserved IPs in Cloud Subnets (AWS & Azure)

In standard on-premise networking, only 2 IPs are reserved per subnet (Network and Broadcast). **In AWS and Azure, 5 IP addresses are reserved in EVERY subnet:**

Example for `10.0.1.0/24`:
- `10.0.1.0`: **Network address** (reserved).
- `10.0.1.1`: **VPC Router** (default gateway).
- `10.0.1.2`: **DNS Server** (AmazonProvidedDNS / Route 53 Resolver).
- `10.0.1.3`: **Reserved by AWS** for future use.
- `10.0.1.255`: **Network Broadcast address** (VPC does not support broadcast, but still reserved).

> [!WARNING]
> A `/28` subnet has 16 total IPs, but only **11 usable host IPs** in AWS/Azure! Never create subnets smaller than `/28` in cloud VPCs.

---

## 2. Multi-Tier Production VPC Design

Standard AWS/Azure VPC blueprint (`10.0.0.0/16`):
```text
Availability Zone A (AZ-1)               Availability Zone B (AZ-2)
+------------------------------------+   +------------------------------------+
| Public Subnet: 10.0.1.0/24 (NAT,ALB)|   | Public Subnet: 10.0.2.0/24 (NAT,ALB)|
+------------------------------------+   +------------------------------------+
| Private App:   10.0.10.0/24 (EC2)  |   | Private App:   10.0.11.0/24 (EC2)  |
+------------------------------------+   +------------------------------------+
| Database:      10.0.20.0/24 (RDS)  |   | Database:      10.0.21.0/24 (RDS)  |
+------------------------------------+   +------------------------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - CIDR Classless Inter Domain Routing](./04-CIDR-Classless-Inter-Domain-Routing.md) | [Index](../../../README.md) | [06 - IPv6 Architecture and Migration →](./06-IPv6-Architecture-and-Migration.md) |
