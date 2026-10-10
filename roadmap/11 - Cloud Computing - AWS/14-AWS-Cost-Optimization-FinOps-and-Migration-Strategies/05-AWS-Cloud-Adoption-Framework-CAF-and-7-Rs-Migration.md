# 05 - Cloud Migration: AWS CAF & The 7 Rs Migration Strategies

> **Focus:** AWS Cloud Adoption Framework (CAF 6 Perspectives), and the definitive 7 Rs cloud migration strategy framework.

---

## 🚚 The 7 Rs Cloud Migration Strategies

| Migration Strategy | Technical Action | Effort & Risk | Value |
| :--- | :--- | :---: | :---: |
| **1. Rehost ("Lift and Shift")** | Move virtual machines exactly as-is from on-premises to EC2 using AWS Application Migration Service (MGN). | Low | Fast ROI, zero code rewrite. |
| **2. Replatform ("Lift, Tinker & Shift")** | Move to cloud with minor optimizations without modifying core code (e.g., move self-managed MySQL to Amazon RDS). | Medium | Reduced management overhead. |
| **3. Repurchase ("Drop and Shop")** | Discard existing legacy software and replace with SaaS (e.g., replace custom CRM with Salesforce). | Low | Instant modernization. |
| **4. Refactor / Re-architect** | Completely rewrite application code using cloud-native serverless, microservices, and managed queues. | **High** | **Maximum agility, scalability & elasticity**. |
| **5. Relocate ("Hypervisor-Level")** | Move VMware vSphere VMs directly to AWS via VMware Cloud on AWS with zero conversions. | Low | Seamless portability. |
| **6. Retain ("Do Nothing Now")** | Keep critical legacy systems on-premises due to compliance or lack of business justification. | None | Avoids unnecessary risk. |
| **7. Retire ("Decommission")** | Identify and turn off legacy, obsolete servers that are no longer actively used by anyone. | None | **Instant 100% cost savings**. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Storage and Data Transfer Cost Optimization](./04-Storage-and-Data-Transfer-Cost-Optimization.md) | [Index](../../../README.md) | [06 - AWS Migration Tools Migration Hub MGN and DMS →](./06-AWS-Migration-Tools-Migration-Hub-MGN-and-DMS.md) |
