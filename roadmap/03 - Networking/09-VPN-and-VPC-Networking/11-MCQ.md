# 11 - VPN & VPC: Self-Assessment MCQs

### Q1. Which AWS VPC endpoint type is completely free of hourly and data processing charges?
- A) Interface Endpoint (PrivateLink)
- B) Gateway Endpoint (S3 / DynamoDB)
- C) Transit Gateway Attachment
- D) VPN Connection Endpoint
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Gateway Endpoints for S3 and DynamoDB are provided by AWS at zero cost.</details>

---

### Q2. In a multi-AZ VPC architecture, what is the best practice for NAT Gateway placement?
- A) One NAT Gateway shared across all private subnets in a single public subnet
- B) One NAT Gateway per Availability Zone deployed inside each public subnet
- C) Deploying NAT Gateways inside private subnets
- D) Attaching an Internet Gateway directly to each private instance
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Deploying one NAT Gateway per AZ ensures AZ-level fault isolation and resilience.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
