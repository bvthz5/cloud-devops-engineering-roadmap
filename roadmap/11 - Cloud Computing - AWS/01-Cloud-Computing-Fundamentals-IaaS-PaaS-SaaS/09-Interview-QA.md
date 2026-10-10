# 09 - Cloud Fundamentals & AWS Architecture: Real-Time Interview Q&A

> **Level:** Beginner $ightarrow$ Advanced | **Focus:** Shared Responsibility Model, Cloud Economics, IaaS vs. PaaS vs. SaaS trade-offs, and FinOps cost governance.

---

### 🟢 Q1. What is the AWS Shared Responsibility Model?
**Answer:** The Shared Responsibility Model defines security boundaries. **AWS is responsible for "Security OF the Cloud"** (hardware, hypervisor, physical datacenter facilities, and core network). **The customer is responsible for "Security IN the Cloud"** (customer data, IAM credentials, operating system updates, firewall security groups, and encryption).

---

### 🟡 Q2. When is on-premises infrastructure cheaper than cloud computing?
**Answer:** On-premises can be cheaper when workloads are **steady-state, predictable, running 24/7 at heavy capacity for multiple years**, and the organization already owns the datacenter hardware. Cloud becomes significantly more expensive if servers are left running idle or large volumes of data are transferred out of the cloud incurring heavy egress bandwidth charges.

---

### 🟡 Q3. What is the difference between IaaS, PaaS, and SaaS?
**Answer:**
- **IaaS (Infrastructure as a Service):** Renting raw compute, storage, and networking (Amazon EC2, EBS). You manage OS, runtime, and application code.
- **PaaS (Platform as a Service):** Managed application hosting environment (AWS Elastic Beanstalk, RDS). AWS manages OS and runtime; you only provide application code and data.
- **SaaS (Software as a Service):** Fully completed software application delivered via web browser (Microsoft 365, Salesforce). You manage zero infrastructure or code.

---

### 🔴 Q4. How does AWS Graviton reduce compute costs?
**Answer:** AWS Graviton processors are custom 64-bit ARM-based CPUs designed by AWS (Annapurna Labs). Because ARM microarchitecture is significantly more power- and thermal-efficient than x86 Intel/AMD chips, AWS passes these savings to customers, offering up to **40% better price-performance** for Linux microservices, container workloads, and open-source databases.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
