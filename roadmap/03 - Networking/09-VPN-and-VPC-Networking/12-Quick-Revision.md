# 12 - VPN & VPC: Quick Revision Cheat Sheet

## Subnet & Route Table Rules

| Target Component | Belongs in Subnet | Default Route (`0.0.0.0/0`) Target |
|---|---|---|
| Public Load Balancer / Bastion | Public Subnet | Internet Gateway (`igw-xxxx`) |
| Private App Server / Microservices | Private Subnet | NAT Gateway (`nat-xxxx`) in same AZ |
| Secure Database (No Internet) | Isolated Private Subnet | None (local routing only) |
| S3 / DynamoDB Traffic | Any Subnet | S3 Gateway Endpoint (`vpce-xxxx`) |

## Quick Security Checklist
- Avoid `0.0.0.0/0` inbound in Security Groups (except ports 80/443 on public ALBs).
- Use S3 Gateway Endpoints to avoid million-dollar NAT bills.
- Keep Security Groups stateful; use NACLs only for coarse-grained IP blacklisting.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [10 - Troubleshooting Tools](../10-Network-Troubleshooting-Tools/README.md) |
