# 04 - AWS Shared Responsibility Model Architecture

> **Focus:** The boundary between AWS responsibilities ("Security OF the Cloud") and Customer responsibilities ("Security IN the Cloud") across IaaS, PaaS, and Serverless/SaaS service models.

---

## 🛡️ 1. The Core Architecture of Shared Responsibility

Security and compliance is a **shared effort** between AWS and the customer:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   CUSTOMER: SECURITY "IN" THE CLOUD                    │
│                                                                        │
│   • Customer Data (Encryption, Access Policies, Classification)        │
│   • Identity & Access Management (IAM Users, MFA, Role Policies)       │
│   • Operating System, Network & Firewall Configuration (Guest OS, SGs) │
│   • Application Code, Runtime Dependencies & Data Integrity            │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ The Security Boundary Line
┌───────────────────────────────────▼────────────────────────────────────┐
│                      AWS: SECURITY "OF" THE CLOUD                      │
│                                                                        │
│   • Software Infrastructure: Compute, Storage, Database, Networking   │
│   • Hardware & AWS Global Infrastructure: Regions, AZs, Edge Locations │
│   • Physical Datacenter Security: Armed guards, biometrics, cameras    │
│   • Environmental Controls: Power redundancy, HVAC cooling, fire flood │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 📊 2. Responsibility Across Service Models

| Operational Layer | IaaS (Amazon EC2, EBS) | PaaS (Amazon RDS, Elastic Beanstalk) | Serverless / Abstracted (S3, DynamoDB, Lambda) |
| :--- | :---: | :---: | :---: |
| **Customer Data** | **Customer** | **Customer** | **Customer** |
| **IAM Access Control** | **Customer** | **Customer** | **Customer** |
| **Application Code** | **Customer** | **Customer** | **Customer** |
| **Database Engine Patching** | **Customer** | **AWS** | **AWS** |
| **Operating System Patching**| **Customer** | **AWS** | **AWS** |
| **Virtualization / Hypervisor**| **AWS** | **AWS** | **AWS** |
| **Physical Hardware & Power** | **AWS** | **AWS** | **AWS** |

---

## ⚠️ 3. Catastrophic Customer Misconceptions

1. *"AWS takes automatic backups of my EC2 instances."* $ightarrow$ **FALSE!** AWS guarantees the physical server runs, but EBS snapshot schedules and disaster recovery backups are 100% the customer's responsibility.
2. *"My data in S3 is automatically private and safe."* $ightarrow$ **FALSE!** If you disable S3 Block Public Access and configure an open bucket policy, your data is exposed. You are responsible for bucket permissions.
3. *"AWS handles OS security patches on my EC2 servers."* $ightarrow$ **FALSE!** AWS provides unpatched base AMIs; updating packages via `yum update` or `apt upgrade` is entirely your duty.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Cloud Deployment Models Public Private Hybrid MultiCloud](./03-Cloud-Deployment-Models-Public-Private-Hybrid-MultiCloud.md) | [Index](../../../README.md) | [05 - Capex vs Opex and Cloud Economics →](./05-Capex-vs-Opex-and-Cloud-Economics.md) |
