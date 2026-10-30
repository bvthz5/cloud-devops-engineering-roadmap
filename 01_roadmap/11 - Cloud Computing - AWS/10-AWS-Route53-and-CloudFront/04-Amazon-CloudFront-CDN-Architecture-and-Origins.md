# 04 - Amazon CloudFront: Global CDN, Origin Shield & Origin Access Control (OAC)

> **Focus:** Edge acceleration, 600+ Points of Presence, Origin Access Control (OAC) for S3 protection, CloudFront Functions vs. Lambda@Edge, and SSL/TLS termination via ACM.

---

## ⚡ 1. Amazon CloudFront Architecture

**Amazon CloudFront** is a global Content Delivery Network (CDN) service that securely delivers data, videos, applications, and APIs to users with low latency and high transfer speeds:

```text
User in London (UK)
        │
        ▼
┌────────────────────────────────────────────────────────┐
│           CloudFront Edge Location (London PoP)        │
│  • Edge Cache HIT ────────► Instant millisecond delivery
│  • Edge Cache MISS ───────┐
└───────────────────────────┼────────────────────────────┘
                            │ Traverses AWS Private Dedicated Backbone
                            ▼
┌────────────────────────────────────────────────────────┐
│               S3 Origin / ALB (us-east-1)              │
│  Protected via Origin Access Control (OAC)             │
│  (Direct public internet access is completely BLOCKED) │
└────────────────────────────────────────────────────────┘
```

---

## 🔒 2. Origin Access Control (OAC) Security

Never leave S3 buckets publicly accessible to serve CDN content. Use **Origin Access Control (OAC)**:
- S3 bucket policy allows read access **only** when requested by the specific CloudFront distribution ARN (`arn:aws:cloudfront::123456789012:distribution/EDFDVBD632BHDS5`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Route53 Health Checks and DNS Failover](./03-Route53-Health-Checks-and-DNS-Failover.md) | [Index](../../../README.md) | [05 - CloudFront Security Signed URLs OAC and Lambda Edge →](./05-CloudFront-Security-Signed-URLs-OAC-and-Lambda-Edge.md) |
