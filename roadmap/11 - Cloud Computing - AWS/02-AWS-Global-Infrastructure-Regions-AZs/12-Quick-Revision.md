# 12 - Quick Revision: AWS Global Infrastructure & Resiliency

| Concept | High-Speed Exam Summary |
| :--- | :--- |
| **Regions** | 34+ geographic areas; each contains at least 3 AZs. |
| **Availability Zones** | 108+ isolated datacenters; $< 1\text{ ms}$ inter-AZ dark fiber links. |
| **AZ ID** | Immutable physical identifier (e.g. `use1-az1`) bypassing account AZ name randomization. |
| **Edge Locations** | 600+ PoPs worldwide powering CloudFront CDN caching and Route 53 DNS. |
| **Global Accelerator**| 2 Anycast IPs routing non-cached TCP/UDP traffic over the AWS fiber backbone. |
| **Local Zones** | Datacenters in major cities delivering single-digit ms latency to local end users. |
| **Outposts** | Physical 42U rack servers running native AWS APIs inside on-premises datacenters. |
| **4 DR Strategies** | Backup & Restore (Hours, $) $\rightarrow$ Pilot Light $\rightarrow$ Warm Standby $\rightarrow$ Multi-Site Active-Active (Real-time, $$$$$). |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (03-AWS-IAM-Users-Roles-Policies) →](../03-AWS-IAM-Users-Roles-Policies/01-AWS-IAM-Architecture-and-Identity-Management.md) |
