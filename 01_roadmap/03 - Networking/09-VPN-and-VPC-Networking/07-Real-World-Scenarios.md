# 07 - VPN & VPC: Real-World Production Scenarios

## Scenario 1: The $45,000 Surprise NAT Gateway Bill

### Incident Summary
A machine learning startup noticed their monthly AWS bill jumped unexpectedly by $45,000. Analysis revealed that EC2 compute instances were under budget, but NAT Gateway data processing charges accounted for 75% of the total cloud spend.

### Root Cause
The data science team was pulling 800 Terabytes of training datasets from AWS S3 buckets into EC2 instances situated in private subnets. Because no S3 Gateway Endpoint was configured in the VPC route tables, every byte of data traversed the NAT Gateway, incurring $0.045 per Gigabyte processed!

### Resolution
Executed a zero-downtime Terraform addition:
```hcl
resource "aws_vpc_endpoint" "s3" {
  vpc_id          = aws_vpc.main.id
  service_name    = "com.amazonaws.us-east-1.s3"
  route_table_ids = [aws_route_table.private.id]
}
```
All S3 traffic instantly bypassed the NAT Gateway and traversed the free AWS internal fabric, reducing the NAT bill from $45,000 to $320 the following month.

---

## Scenario 2: Overlapping Subnets in M&A Corporate Takeover

### Incident Summary
Company A acquired Company B. Company A's core Kubernetes cluster was in `10.100.0.0/16`. Company B's production database was also in `10.100.0.0/16`. Direct VPC peering was rejected by AWS due to overlapping CIDRs.

### Resolution
Rather than undergoing a 6-month database migration to re-IP Company B's VPC, the architecture team deployed an **AWS PrivateLink endpoint service**. Company B exposed the database behind an AWS Network Load Balancer (NLB) attached to an Endpoint Service. Company A provisioned an Interface Endpoint in their VPC, receiving a private IP from their local CIDR (`10.100.5.42`) that cleanly routed to Company B without network collision.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Zero Trust Network Access ZTNA vs Perimeter VPN](./06-Zero-Trust-Network-Access-ZTNA-vs-Perimeter-VPN.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
