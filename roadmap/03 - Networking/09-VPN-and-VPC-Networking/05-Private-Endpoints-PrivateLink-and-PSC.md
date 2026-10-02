# 05 - PrivateLink, VPC Endpoints, and Private Service Connect

## 1. The NAT Gateway Egress Problem

By default, when a private EC2 instance or Kubernetes pod accesses a cloud service like AWS S3, DynamoDB, or GitHub:
1. Traffic leaves the private subnet.
2. Traverses the NAT Gateway.
3. Hits the public AWS S3 endpoint over the internet.
4. **Massive Cloud Cost:** AWS charges $0.045/GB for NAT Gateway data processing! High-volume data lakes incur tens of thousands of dollars monthly in NAT fees alone.

---

## 2. Gateway Endpoints vs. Interface Endpoints

| Type | How it Works | AWS Services Supported | Cost |
|---|---|---|---|
| **Gateway Endpoint** | Injects an entry into the subnet Route Table pointing to a virtual prefix list | **S3** and **DynamoDB** only | **FREE** (No hourly fee, no data transfer fee) |
| **Interface Endpoint (PrivateLink)** | Provisions an Elastic Network Interface (ENI) with a private IP directly inside your private subnet | 100+ AWS services (ECR, Secrets Manager, SQS, STS, custom SaaS) | Hourly fee (~$0.01/hr per AZ) + $0.01/GB data processed |

---

## 3. GCP Private Service Connect (PSC) & Azure Private Link
In GCP and Azure, Private Service Connect / Private Link allows exposing internal microservices across different organizational VPCs without peering or IP space overlap.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Transit Gateways](./04-Transit-Gateways-and-Cloud-Interconnects.md) | [README](./README.md) | [06 - Zero Trust vs VPN](./06-Zero-Trust-Network-Access-ZTNA-vs-Perimeter-VPN.md) |
