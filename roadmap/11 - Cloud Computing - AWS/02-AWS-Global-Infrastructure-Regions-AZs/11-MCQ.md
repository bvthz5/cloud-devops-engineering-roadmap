# 11 - Multiple Choice Practice Questions: Global Infrastructure

---

### 1. Which AWS global infrastructure component provides static Anycast IP addresses to route TCP/UDP traffic across the AWS global backbone without caching?
- [ ] A) Amazon CloudFront
- [x] B) AWS Global Accelerator
- [ ] C) Amazon Route 53
- [ ] D) AWS Direct Connect
> **Explanation:** AWS Global Accelerator assigns two static Anycast IP addresses and routes traffic directly across the AWS private global fiber network without edge caching.

---

### 2. A solutions architect needs to ensure that two AWS accounts share subnets inside the exact same physical datacenter in us-east-1. What should they compare?
- [ ] A) Availability Zone Name (`us-east-1a`)
- [x] B) Availability Zone ID (`use1-az1`)
- [ ] C) VPC ID
- [ ] D) Subnet CIDR Block
> **Explanation:** AZ names are mapped randomly to different physical datacenters per account. The immutable AZ ID identifies the true physical datacenter facility.

---

### 3. What is the minimum number of Availability Zones contained inside an AWS Region?
- [ ] A) 1
- [ ] B) 2
- [x] C) 3
- [ ] D) 6
> **Explanation:** Every standard launched AWS Region contains at least 3 physically separate Availability Zones to ensure high availability.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
