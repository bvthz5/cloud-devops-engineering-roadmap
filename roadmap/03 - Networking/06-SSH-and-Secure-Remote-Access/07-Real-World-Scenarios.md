# 07 — Real-World SSH Production Scenarios

---

## Scenario 1: Accessing Private VPC Database via Bastion Tunnel

Connect your local pgAdmin / DBeaver to an RDS PostgreSQL database residing in a private subnet with zero internet access:
```bash
ssh -N -L 5432:aurora-cluster.internal.company:5432 ec2-user@bastion.company.com
```
Point DBeaver to `127.0.0.1:5432`!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - SSH Certificates and Zero Trust Access](./06-SSH-Certificates-and-Zero-Trust-Access.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
