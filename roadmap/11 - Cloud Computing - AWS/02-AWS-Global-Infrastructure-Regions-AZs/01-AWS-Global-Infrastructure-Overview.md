# 01 - AWS Global Infrastructure Architecture Overview

> **Focus:** Physical global datacenter footprint, Regions, Availability Zones (AZs), Local Zones, AWS Wavelength, Edge Locations (Points of Presence - PoPs), and Regional Services vs. Global Services.

---

## 🌍 1. The Global Footprint Hierarchy

AWS operates the largest cloud infrastructure on Earth. The physical layout is organized into a strict geometric hierarchy:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        AWS GLOBAL INFRASTRUCTURE                       │
│                                                                        │
│   ┌────────────────────────────────────────────────────────────────┐   │
│   │ REGIONS (Geographic areas around the globe e.g., us-east-1)   │   │
│   │   ├── Availability Zone A (Isolated Datacenter complex)       │   │
│   │   ├── Availability Zone B (Isolated Datacenter complex)       │   │
│   │   └── Availability Zone C (Isolated Datacenter complex)       │   │
│   └────────────────────────────────────────────────────────────────┘   │
│                                                                        │
│   ┌───────────────────────────┐      ┌─────────────────────────────┐   │
│   │ EDGE LOCATIONS (PoPs)     │      │ SPECIALIZED EXTENSIONS      │   │
│   │ • 600+ CloudFront CDN     │      │ • Local Zones (Metro edge)  │   │
│   │ • Route 53 Anycast DNS    │      │ • AWS Wavelength (5G telco) │   │
│   │ • AWS Global Accelerator  │      │ • AWS Outposts (On-premise) │   │
│   └───────────────────────────┘      └─────────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🏛️ 2. Regional Services vs. Global Services

AWS services are architecturally divided by scope:

| Scope | Architectural Meaning | Key AWS Services | Disaster Recovery Considerations |
| :--- | :--- | :--- | :--- |
| **Global Services** | Single global control plane. Data is replicated worldwide automatically; no region selection required. | **IAM**, **Route 53**, **CloudFront**, **WAF**, **AWS Organizations**. | If the global control plane experiences degraded performance (historically tied to `us-east-1`), data plane operations still function locally. |
| **Regional Services** | Tied to a specific geographic region (e.g., `eu-west-1`). You choose where resources and data reside. | **Amazon S3**, **Amazon VPC**, **Amazon RDS**, **AWS Lambda**, **Amazon DynamoDB**. | Requires explicit multi-region architecture (e.g., S3 Cross-Region Replication, Aurora Global Database) for cross-continental failover. |
| **Zonal Services** | Tied to a specific physical Availability Zone inside a region. | **Amazon EC2**, **Amazon EBS Volumes**, **Subnets**. | A failure in a single datacenter building will terminate zonal resources unless managed by an Auto Scaling Group across multiple AZs. |

---

## 📍 3. Edge Locations, Local Zones & AWS Wavelength

- **Edge Locations (Points of Presence - PoP):** 600+ caching nodes located in major metropolitan areas worldwide. Used by **Amazon CloudFront** to cache videos, images, and static assets near users, and **Route 53** to answer DNS requests within single-digit milliseconds.
- **Local Zones:** Datacenters situated in cities without a full AWS Region (e.g., Chicago, Los Angeles, Buenos Aires). Extends VPCs to deliver single-digit millisecond latency to local end users for gaming, media production, and video streaming.
- **AWS Wavelength:** Embeds AWS compute and storage hardware directly inside telecommunications 5G networks (Verizon, Vodafone, KDDI). Allows mobile applications to connect to compute without leaving the mobile carrier network (ultra-low latency for autonomous vehicles, smart factories, AR/VR).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (01-Cloud-Computing-Fundamentals-IaaS-PaaS-SaaS)](../01-Cloud-Computing-Fundamentals-IaaS-PaaS-SaaS/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - AWS Regions and Availability Zones Architecture →](./02-AWS-Regions-and-Availability-Zones-Architecture.md) |
