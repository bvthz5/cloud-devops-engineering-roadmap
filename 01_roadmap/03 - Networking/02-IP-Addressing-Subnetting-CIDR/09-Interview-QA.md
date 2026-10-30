# 09 — IP Addressing & Subnetting Interview Q&A

10 technical interview questions for Cloud, DevOps, and SRE roles.

---

### Q1: Why does AWS reserve 5 IP addresses in every VPC subnet instead of the standard 2?
**Answer:**
Standard networking reserves 2 addresses: the **Network address** (first IP) and the **Broadcast address** (last IP). AWS reserves an additional 3 IPs in every subnet:
1. `Network + 1`: Reserved for the VPC default router.
2. `Network + 2`: Reserved for AmazonProvidedDNS (Route 53 Resolver).
3. `Network + 3`: Reserved by AWS for future internal use.

---

### Q2: What is the purpose of the link-local address `169.254.169.254` in cloud environments?
**Answer:**
`169.254.169.254` is the universal IP for the **Instance Metadata Service (IMDS)** in AWS, GCP, Azure, and OpenStack. A VM or container makes an HTTP request to this link-local endpoint to retrieve runtime instance details (instance ID, private IP, AMI ID) and temporary security credentials (IAM role tokens).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
