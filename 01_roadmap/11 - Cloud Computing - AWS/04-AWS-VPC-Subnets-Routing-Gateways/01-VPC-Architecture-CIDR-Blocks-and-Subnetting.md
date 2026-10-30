# 01 - Amazon VPC Architecture, CIDR Blocks & Subnetting

> **Focus:** Software-defined networking, VPC CIDR blocks, the 5 reserved IP addresses in AWS subnets, host capacity calculations, and Public vs. Private subnet boundaries.

---

## 🌐 1. Amazon Virtual Private Cloud (VPC) Architecture

An **Amazon VPC** is a logically isolated virtual network dedicated to your AWS account. It gives you complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   VIRTUAL PRIVATE CLOUD (10.0.0.0/16)                  │
│                                                                        │
│  ┌───────────────────────┐                    ┌─────────────────────┐  │
│  │   PUBLIC SUBNET       │                    │   PRIVATE SUBNET    │  │
│  │   (10.0.1.0/24)       │                    │   (10.0.2.0/24)     │  │
│  │   Availability Zone A │                    │ Availability Zone A │  │
│  │                       │                    │                     │  │
│  │   [Web / ALB Proxy]   │                    │ [Backend Database]  │  │
│  │   Public IP Assigned  │                    │ Private IP Only     │  │
│  └───────────┬───────────┘                    └───────────▲─────────┘  │
│              │                                            │            │
│              │ (Traffic to 0.0.0.0/0)                     │            │
│              ▼                                            │            │
│     [Internet Gateway]                     [NAT Gateway] ─┘            │
│     (Bidirectional Door)                   (Outbound-Only Updates)     │
└──────────────┬─────────────────────────────────────────────────────────┘
               ▼
         PUBLIC INTERNET
```

---

## 🔢 2. The 5 Reserved IP Addresses in AWS Subnets

In every AWS subnet, AWS reserves the **first 4 IP addresses and the last 1 IP address** (5 total IPs per subnet). They cannot be assigned to any EC2 instance or container:

| IP Address Offset | Example (`10.0.1.0/24`) | Technical Reserved Purpose |
| :---: | :--- | :--- |
| **`0`** | `10.0.1.0` | **Network Address** (Identifies the wire network). |
| **`1`** | `10.0.1.1` | **VPC Router Default Gateway** (Handles routing out of the subnet). |
| **`2`** | `10.0.1.2` | **Amazon Route 53 DNS Resolver** (Provides internal VPC DNS resolution). |
| **`3`** | `10.0.1.3` | Reserved by AWS for future functionality. |
| **`255`** | `10.0.1.255` | **Network Broadcast Address** (AWS VPC does not support broadcast, but address is reserved). |

### 🧮 Subnet Usable Host Calculation Formula:
$$	ext{Usable Hosts} = 2^{(32 - 	ext{CIDR Prefix})} - 5$$
- For a `/24` subnet: $2^{(32 - 24)} - 5 = 256 - 5 = mathbf{251	ext{ usable IP addresses}}$.
- For a `/28` subnet (AWS minimum size): $2^{(32 - 28)} - 5 = 16 - 5 = mathbf{11	ext{ usable IP addresses}}$.

---

### 💻 Production AWS CLI VPC Creation

```bash
# 1. Create a VPC with CIDR 10.0.0.0/16
VPC_ID=$(aws ec2 create-vpc \
  --cidr-block 10.0.0.0/16 \
  --query 'Vpc.VpcId' --output text)

# 2. Enable DNS Hostnames
aws ec2 modify-vpc-attribute --vpc-id $VPC_ID --enable-dns-hostnames "{\"Value\":true}"

# 3. Create Public Subnet in us-east-1a
aws ec2 create-subnet \
  --vpc-id $VPC_ID \
  --cidr-block 10.0.1.0/24 \
  --availability-zone us-east-1a
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (03-AWS-IAM-Users-Roles-Policies)](../03-AWS-IAM-Users-Roles-Policies/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Internet Gateways NAT Gateways and Egress Only IGW →](./02-Internet-Gateways-NAT-Gateways-and-Egress-Only-IGW.md) |
