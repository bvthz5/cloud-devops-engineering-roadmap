# 03 - Edge Locations, Regional Edge Caches, CloudFront & AWS Global Accelerator

> **Focus:** Edge network topology, 600+ Points of Presence (PoPs), Regional Edge Caches (RECs), Anycast IP routing, and CloudFront vs. AWS Global Accelerator.

---

## 🌐 1. Edge Topology: PoPs vs. Regional Edge Caches

AWS deploys a multi-tiered global caching hierarchy to accelerate content delivery:

```text
End User (Sydney, Australia)
          │
          ▼
┌────────────────────────────────────────────────────────┐
│           Edge Location (Sydney Point of Presence)     │
│  • 600+ locations worldwide                            │
│  • Direct low-latency termination of TCP/TLS           │
│  • Fast Cache HIT ──► Instant response to user         │
└───────────────────────────┬────────────────────────────┘
                            │ Cache MISS
                            ▼
┌────────────────────────────────────────────────────────┐
│             Regional Edge Cache (REC)                  │
│  • 30+ massive regional caching centers worldwide      │
│  • Retains rarely accessed long-tail content           │
└───────────────────────────┬────────────────────────────┘
                            │ Cache MISS (Travels over AWS private fiber)
                            ▼
┌────────────────────────────────────────────────────────┐
│               Origin Server (us-east-1)                │
│             Amazon S3 Bucket / ALB Backend             │
└────────────────────────────────────────────────────────┘
```

---

## ⚖️ 2. Amazon CloudFront vs. AWS Global Accelerator

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│        Amazon CloudFront (CDN)       │       │        AWS Global Accelerator        │
│                                      │       │                                      │
│  • OSI Layer 7 (HTTP / HTTPS)        │       │  • OSI Layer 4 & Layer 7 (TCP / UDP) │
│  • CACHES data at the Edge (CDN)     │       │  • NO CACHING! Pure network transport│
│  • Best for static files, media,     │       │  • Provides 2 STATIC ANYCAST IPv4s   │
│    videos, HTML, dynamic API caching │       │  • Best for gaming, VoIP, IoT,       │
│  • Integrated with AWS WAF           │       │    non-HTTP TCP/UDP protocols        │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - AWS Regions and Availability Zones Architecture](./02-AWS-Regions-and-Availability-Zones-Architecture.md) | [Index](../../../README.md) | [04 - Resiliency High Availability and Disaster Recovery →](./04-Resiliency-High-Availability-and-Disaster-Recovery.md) |
