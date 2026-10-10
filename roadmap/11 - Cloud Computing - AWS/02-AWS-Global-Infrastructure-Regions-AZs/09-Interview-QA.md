# 09 - AWS Global Infrastructure: Real-Time Interview Q&A

> **Level:** Beginner $ightarrow$ Advanced | **Focus:** Regions, AZs, Edge Locations, AZ ID mapping, and High Availability vs. Disaster Recovery.

---

### 🟢 Q1. What is the difference between a Region and an Availability Zone?
**Answer:** An **AWS Region** is a physical geographical location in the world containing multiple, physically separated and isolated datacenters. An **Availability Zone (AZ)** is one or more discrete datacenters within a region equipped with redundant power, networking, and cooling. Regions provide geographic dispersion; AZs provide fault-tolerant high availability within a region.

---

### 🟡 Q2. What is an AZ ID and why is it critical in multi-account organizations?
**Answer:** AWS coordinates customer workload distribution by randomly mapping AZ names (`us-east-1a`, `us-east-1b`) to physical datacenters per account. An **AZ ID** (e.g., `use1-az1`) is a static, immutable identifier across all AWS accounts. In multi-account enterprise organizations sharing VPC subnets, teams must use AZ IDs to ensure resources actually reside in the same physical datacenter for low-latency communication.

---

### 🔴 Q3. When would you choose AWS Global Accelerator over Amazon CloudFront?
**Answer:**
- Choose **Amazon CloudFront** for **cacheable HTTP/HTTPS web content**, video streaming, and static assets.
- Choose **AWS Global Accelerator** for **non-HTTP protocols (TCP/UDP, VoIP, gaming)** or applications requiring **2 static Anycast public IP addresses** that route traffic directly over AWS's private global fiber network without caching.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
