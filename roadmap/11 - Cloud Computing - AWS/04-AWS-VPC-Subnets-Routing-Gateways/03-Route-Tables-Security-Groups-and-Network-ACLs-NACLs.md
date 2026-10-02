# 03 - Security Groups vs Network ACLs (NACLs)

| Feature | Security Group (SG) | Network ACL (NACL) |
|---|---|---|
| **Level** | Instance / ENI level | Subnet level |
| **Stateful?** | **YES** (Return traffic automatically allowed) | **NO** (Inbound & Outbound rules required) |
| **Rules** | Allow rules ONLY | Allow and Deny rules |
| **Evaluation** | Evaluates ALL rules before decision | Evaluates rules in numerical order |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Internet Gateways NAT Gateways and Egress Only IGW](./02-Internet-Gateways-NAT-Gateways-and-Egress-Only-IGW.md) | [Index](../../../README.md) | [04 - VPC Peering and AWS Transit Gateway →](./04-VPC-Peering-and-AWS-Transit-Gateway.md) |
