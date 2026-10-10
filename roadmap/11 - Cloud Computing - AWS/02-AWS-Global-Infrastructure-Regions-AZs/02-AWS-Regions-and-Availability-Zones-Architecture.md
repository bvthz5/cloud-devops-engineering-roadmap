# 02 - AWS Regions & Availability Zones Architecture

> **Focus:** Geographic region design, AZ physical separation, inter-AZ low-latency fiber links, choosing the right region, and AZ Name vs. AZ ID mapping across AWS accounts.

---

## 🌐 1. AWS Region Architecture

An **AWS Region** is a physical geographic location in the world with multiple, physically separated and isolated Availability Zones:

```text
Region: us-east-1 (N. Virginia)
┌────────────────────────────────────────────────────────────────────────┐
│  ┌───────────────────────┐  Low-Latency (<1ms) ┌─────────────────────┐ │
│  │ Availability Zone 1a  │ ◄── Metro Fiber ──► │Availability Zone 1b │ │
│  │ (Independent Power,   │                     │(Independent Power,  │ │
│  │  Cooling & Network)   │                     │ Cooling & Network)  │ │
│  └──────────┬────────────┘                     └──────────┬──────────┘ │
│             │                                             │            │
│             └───────────────────┬─────────────────────────┘            │
│                        Low-Latency Fiber                               │
│                                 ▼                                      │
│                    ┌─────────────────────────┐                         │
│                    │ Availability Zone 1c    │                         │
│                    └─────────────────────────┘                         │
└────────────────────────────────────────────────────────────────────────┘
```

### 🎯 4 Factors for Selecting an AWS Region:
1. **Compliance & Legal Governance:** Laws like GDPR (Europe), HIPAA (US), or DPDP (India) dictate that user data must physically remain within national borders.
2. **Proximity & Network Latency:** Choose the region closest to the majority of your user base to minimize packet transit time.
3. **Service Availability:** Not every AWS service is available in every region on day one (new AI models and instances debut first in `us-east-1`, `us-west-2`, and `eu-west-1`).
4. **Cost Discrepancy:** Power, real estate, and local taxes mean running an EC2 instance in São Paulo (`sa-east-1`) can cost up to 40% more than Northern Virginia (`us-east-1`).

---

## 🏢 2. Availability Zone Architecture & AZ ID Mapping

- **Physical Separation:** AZs are typically located 10 to 60 miles apart—far enough to ensure a localized flood, power grid failure, or storm cannot knock out two zones, but close enough to enable synchronous replication with **sub-millisecond latency**.
- **The AZ Randomization Problem:** To prevent all customers from crowding into `us-east-1a`, AWS randomly maps AZ letters to physical datacenters per account!
  - In Account A: `us-east-1a` might be physical datacenter **use1-az1**.
  - In Account B: `us-east-1a` might be physical datacenter **use1-az4**.
- **AZ ID Solution:** Use the immutable **AZ ID** (`use1-az1`, `use1-az2`) when coordinating shared VPCs and resources across multiple AWS accounts!

```bash
# Inspect AZ Name to AZ ID mapping via AWS CLI
aws ec2 describe-availability-zones \
  --region us-east-1 \
  --query "AvailabilityZones[].{Name:ZoneName, ID:ZoneId}" \
  --output table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - AWS Global Infrastructure Overview](./01-AWS-Global-Infrastructure-Overview.md) | [Index](../../../README.md) | [03 - Edge Locations CloudFront and AWS Global Accelerator →](./03-Edge-Locations-CloudFront-and-AWS-Global-Accelerator.md) |
