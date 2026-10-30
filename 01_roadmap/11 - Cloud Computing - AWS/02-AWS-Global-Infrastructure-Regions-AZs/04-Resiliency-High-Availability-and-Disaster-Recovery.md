# 04 - High Availability (HA) & Disaster Recovery (DR) Strategies

> **Focus:** RTO vs. RPO, High Availability (Multi-AZ) vs. Disaster Recovery (Multi-Region), and the 4 Cloud Disaster Recovery Strategies (Backup & Restore, Pilot Light, Warm Standby, Multi-Site Active-Active).

---

## ⏱️ 1. Recovery Point Objective (RPO) vs. Recovery Time Objective (RTO)

```text
Disaster Strikes
       │
       ▼
┌────────────────────────────────────────────────────────┐
│◄──── RPO ────►│◄──────────────── RTO ─────────────────►│
│               │                                        │
│ Last Usable   │ Outage Occurs                   System │
│ Backup Point  │                               Restored │
└────────────────────────────────────────────────────────┘
```

- **RPO (Recovery Point Objective):** The maximum acceptable data loss measured in time. "If disaster strikes at 2:00 PM and we restore from a 1:00 PM snapshot, RPO is 1 hour."
- **RTO (Recovery Time Objective):** The maximum acceptable system downtime. "How long before the application is operational again."

---

## 🏛️ 2. The 4 AWS Disaster Recovery Strategies

```text
Cost: LOWEST ◄──────────────────────────────────────────────────────────► HIGHEST
RPO/RTO: HOURS ◄─────────────────────────────────────────────────────────► SECONDS / ZERO

┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│ BACKUP & RESTORE │  │   PILOT LIGHT    │  │   WARM STANDBY   │  │   MULTI-SITE     │
│                  │  │                  │  │                  │  │  ACTIVE-ACTIVE   │
│ Snapshots sent   │  │ Core data DB     │  │ Scaled-down      │  │ Full production  │
│ to S3 in DR.     │  │ replicated live; │  │ duplicate stack  │  │ active in two+   │
│ Spin up on fail. │  │ compute is 0.    │  │ always running.  │  │ regions live.    │
│ RTO: Hours       │  │ RTO: 10-30 Mins  │  │ RTO: Minutes     │  │ RTO: Near Zero   │
└──────────────────┘  └──────────────────┘  └──────────────────┘  └──────────────────┘
```

| Strategy | Architecture Overview | RPO | RTO | Cost Ratio |
| :--- | :--- | :---: | :---: | :---: |
| **Backup & Restore** | Daily EBS/RDS backups replicated to secondary region via AWS Backup. EC2 spun up via Terraform only after disaster. | Hours | Hours | **$** (Cheapest) |
| **Pilot Light** | Databases continuously replicated live (Aurora Global Database); web/app servers exist only as AMIs and spin up on failover. | Minutes | 10–30 Mins | **$$** |
| **Warm Standby** | Scaled-down minimum working copy of the entire application running 24/7 in secondary region. Auto-scales on DNS switch. | Seconds | Minutes | **$$$** |
| **Multi-Site Active-Active**| Full production capacity running simultaneously in 2+ AWS regions. Traffic routed globally via Route 53. | **Near 0** | **Real-Time** | **$$$$$** (Expensive) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Edge Locations CloudFront and AWS Global Accelerator](./03-Edge-Locations-CloudFront-and-AWS-Global-Accelerator.md) | [Index](../../../README.md) | [05 - Data Sovereignty Compliance and Latency Optimization →](./05-Data-Sovereignty-Compliance-and-Latency-Optimization.md) |
