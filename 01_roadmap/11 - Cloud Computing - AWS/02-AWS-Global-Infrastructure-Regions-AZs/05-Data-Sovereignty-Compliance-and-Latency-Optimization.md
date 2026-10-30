# 05 - Data Sovereignty, Regulatory Compliance & Local Zones

> **Focus:** Data residency legislation (GDPR, DPDP, HIPAA, FedRAMP), AWS Local Zones, Dedicated Local Zones, and compliance architecture patterns.

---

## 🏛️ 1. Global Data Sovereignty Laws

Data Sovereignty specifies that digital data is subject to the legal jurisdictions and governance laws of the physical nation in which it is stored:

| Compliance Framework | Jurisdiction | Strict Cloud Requirement | AWS Architectural Solution |
| :--- | :--- | :--- | :--- |
| **GDPR** | European Union | Personal citizen data must not leave the EU without strict safeguards. | Deploy exclusively to Frankfurt (`eu-central-1`), Ireland (`eu-west-1`), Paris, or Stockholm. |
| **DPDP Act** | India | Critical and sensitive financial/personal data must be stored locally. | Deploy within Mumbai (`ap-south-1`) or Hyderabad (`ap-south-2`). |
| **FedRAMP High / ITAR**| United States | Defense and classified government workloads require screened US citizen operators. | Deploy to **AWS GovCloud (US-East / US-West)**. |

---

## 📍 2. AWS Local Zones: Metro Edge Expansion

**AWS Local Zones** place compute, storage, and database services close to large population and IT centers that do not have a full AWS Region (e.g., Los Angeles, Chicago, Dallas, Taipei, Delhi).
- Allows applications to deliver **single-digit millisecond latency** for real-time video streaming, 3D CAD modeling, and healthcare analytics.
- Controlled as a child subnet extension of a parent AWS Region.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Resiliency High Availability and Disaster Recovery](./04-Resiliency-High-Availability-and-Disaster-Recovery.md) | [Index](../../../README.md) | [06 - AWS Outposts and Hybrid Cloud Infrastructure →](./06-AWS-Outposts-and-Hybrid-Cloud-Infrastructure.md) |
