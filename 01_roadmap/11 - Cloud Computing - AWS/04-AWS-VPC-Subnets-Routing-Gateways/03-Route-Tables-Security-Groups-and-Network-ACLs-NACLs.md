# 03 - Route Tables, Security Groups (Stateful) vs. Network ACLs (Stateless)

> **Focus:** VPC packet filtering, Security Groups vs. Network ACLs (NACLs), Stateful vs. Stateless packet evaluation, Ephemeral port ranges, and Route Table propagation.

---

## ⚖️ 1. Security Groups vs. Network ACLs (NACLs)

```text
Incoming Internet Packet
            │
            ▼
┌────────────────────────────────────────────────────────┐
│             Network ACL (Subnet Boundary)              │
│  • Evaluated FIRST at the subnet perimeter             │
│  • STATELESS: Outbound return traffic checked!         │
│  • Numbered rules evaluated in ascending order         │
└───────────────────────────┬────────────────────────────┘
                            │ (If Allowed by NACL)
                            ▼
┌────────────────────────────────────────────────────────┐
│             Security Group (Instance Virtual NIC)      │
│  • Evaluated SECOND at the virtual machine level       │
│  • STATEFUL: Return response traffic auto-allowed!     │
│  • Only ALLOW rules supported (Implicit deny)          │
└───────────────────────────┬────────────────────────────┘
                            ▼
                    EC2 Virtual Machine
```

### 📊 Master Architectural Comparison:

| Feature | Security Group (SG) | Network Access Control List (NACL) |
| :--- | :--- | :--- |
| **Operating Level** | **Instance / ENI Level** (Virtual Network Card) | **Subnet Level** (Subnet perimeter) |
| **State Tracking** | **Stateful**: Inbound allowed = return outbound auto-allowed! | **Stateless**: Inbound and outbound must be explicitly allowed. |
| **Rule Types** | **ALLOW rules only**. Implicit deny on anything else. | **ALLOW and DENY rules** supported. |
| **Rule Processing** | All rules evaluated simultaneously before making decision. | Processed in **numbered order** (e.g., 100, 200, `*`). |
| **Ephemeral Ports** | Handled automatically by connection tracking table. | **Must explicitly permit ports `1024–65535` outbound!** |
| **Default Setting** | New SG blocks all inbound; allows all outbound. | Default NACL allows all inbound & outbound traffic. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Internet Gateways NAT Gateways and Egress Only IGW](./02-Internet-Gateways-NAT-Gateways-and-Egress-Only-IGW.md) | [Index](../../../README.md) | [04 - VPC Peering and AWS Transit Gateway →](./04-VPC-Peering-and-AWS-Transit-Gateway.md) |
