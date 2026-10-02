# 01 - VPC Architecture & Subnetting

An AWS Virtual Private Cloud (VPC) is an isolated virtual network within an AWS Region.

```text
VPC CIDR: 10.0.0.0/16 (65,536 IPs)
 ├── Public Subnet 1  : 10.0.1.0/24 (us-east-1a)
 ├── Public Subnet 2  : 10.0.2.0/24 (us-east-1b)
 ├── Private Subnet 1 : 10.0.10.0/24 (us-east-1a)
 └── Private Subnet 2 : 10.0.20.0/24 (us-east-1b)
```

AWS reserves 5 IP addresses per subnet (first 4 and last 1).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (03-AWS-IAM-Users-Roles-Policies)](../03-AWS-IAM-Users-Roles-Policies/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Internet Gateways NAT Gateways and Egress Only IGW →](./02-Internet-Gateways-NAT-Gateways-and-Egress-Only-IGW.md) |
