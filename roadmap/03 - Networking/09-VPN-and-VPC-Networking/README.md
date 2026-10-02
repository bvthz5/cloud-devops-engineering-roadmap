# Module 09: VPN and Cloud VPC Networking

Welcome to **Module 09: VPN and Cloud VPC Networking**. Secure network topologies, hybrid cloud connectivity, and isolated multi-tenant virtual networks are fundamental pillars of modern DevOps and Cloud Infrastructure Engineering.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Master VPN tunneling protocols: **IPsec** (IKEv2, ESP/AH), modern **WireGuard**, and **OpenVPN**.
2. Design highly available **Cloud VPCs** across multiple availability zones using public subnets, private subnets, NAT Gateways, and route tables.
3. Understand the mechanics and limitations of **VPC Peering** (non-transitive routing, CIDR collision hazards).
4. Scale enterprise hub-and-spoke architectures using **AWS Transit Gateway (TGW)** and **Azure Virtual WAN**.
5. Eliminate public internet egress using **AWS PrivateLink**, **VPC Endpoints**, and **GCP Private Service Connect**.
6. Transition from legacy perimeter VPNs to modern **Zero Trust Network Access (ZTNA)**.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [VPN Protocols: IPsec, WireGuard & OpenVPN](./01-VPN-Protocols-IPsec-WireGuard-and-OpenVPN.md) | Cryptographic handshakes, tunnel vs transport mode, WireGuard Noise protocol |
| 02 | [Cloud VPC Architecture & Subnet Topology](./02-Cloud-VPC-Architecture-and-Subnet-Topology.md) | Multi-AZ design, public/private subnets, IGW, NAT Gateways, route tables |
| 03 | [VPC Peering & Non-Transitive Routing](./03-VPC-Peering-Architecture-and-Limitations.md) | 1-to-1 peering, routing constraints, solving overlapping CIDR blocks |
| 04 | [Transit Gateways & Hub-and-Spoke Topology](./04-Transit-Gateways-and-Cloud-Interconnects.md) | AWS TGW, route propagation, full-mesh enterprise interconnects |
| 05 | [PrivateLink, VPC Endpoints & Service Connect](./05-Private-Endpoints-PrivateLink-and-PSC.md) | Interface endpoints, Gateway endpoints, private SaaS consumption without NAT |
| 06 | [Zero Trust Network Access (ZTNA) vs. VPN](./06-Zero-Trust-Network-Access-ZTNA-vs-Perimeter-VPN.md) | Identity-aware proxies, Tailscale mesh, BeyondCorp model |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | The $50,000 NAT Gateway bill, overlapping CIDRs during corporate merger |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Asymmetric routing drops, Security Group vs NACL conflicts, IPsec MTU blackholes |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Building a secure WireGuard site-to-site tunnel on Linux |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | VPC design cheat sheet, routing rules summary, VPN protocol matrix |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Load Balancers](../08-Load-Balancers-and-Proxies/README.md) | [Networking Index](../README.md) | [01 - VPN Protocols](./01-VPN-Protocols-IPsec-WireGuard-and-OpenVPN.md) |
