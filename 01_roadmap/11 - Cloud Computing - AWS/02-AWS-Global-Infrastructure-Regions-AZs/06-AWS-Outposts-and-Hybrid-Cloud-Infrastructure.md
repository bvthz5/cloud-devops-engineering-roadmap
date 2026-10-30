# 06 - AWS Outposts, AWS Wavelength & Hybrid Cloud Infrastructure

> **Focus:** True hybrid cloud architecture, AWS Outposts hardware racks, AWS Wavelength for 5G mobile networks, and the AWS Snow Family for edge data transport.

---

## 🏢 1. AWS Outposts Architecture

**AWS Outposts** brings native AWS hardware, services, APIs, and operational models directly to virtually any on-premises datacenter or colocation facility:

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│        AWS Cloud Region (Parent)     │       │     Customer On-Premises Datacenter  │
│                                      │       │                                      │
│  AWS Management Console & Control    │ ◄───► │  [AWS Outpost 42U Server Rack]       │
│  Plane manages both environments     │ VPN / │  • EC2, EBS, ECS, EKS running locally│
│  seamlessly                          │ DX    │  • Sub-millisecond local LAN latency │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

- **Form Factors:** Full 42U rack units (expandable up to 96 racks) or 1U/2U rack-mountable servers for space-constrained edge locations.
- **Use Cases:** Manufacturing assembly lines with $< 1	ext{ ms}$ robotic response requirements, hospital medical imaging processing, and legacy mainframes that cannot be migrated over the WAN.

---

## ❄️ 2. The AWS Snow Family for Edge Data & Disconnected Compute

- **AWS Snowcone:** Ultra-portable, ruggedized 4.5 lb device (8 TB SSD). Can be transported by drone for edge computing.
- **AWS Snowball Edge:** Heavy-duty hardened container (80 TB storage or GPU compute). Used for remote defense operations and marine research.
- **AWS Snowmobile:** A 45-foot ruggedized shipping container pulled by a semi-trailer truck capable of transporting up to **100 Petabytes (PB)** of data for massive datacenter evacuations.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Data Sovereignty Compliance and Latency Optimization](./05-Data-Sovereignty-Compliance-and-Latency-Optimization.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
