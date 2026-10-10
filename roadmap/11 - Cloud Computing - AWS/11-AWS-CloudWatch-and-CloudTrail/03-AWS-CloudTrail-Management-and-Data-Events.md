# 03 - AWS CloudTrail: Governance, Auditing, Management vs. Data Events

> **Focus:** AWS account governance, recording all API calls (Who, What, Where, When), Management Events vs. Data Events, Multi-Region trails, and CloudTrail Insights anomaly detection.

---

## 📜 1. What is AWS CloudTrail?

**AWS CloudTrail** records every single API request executed in your AWS account—whether initiated via the AWS Management Console, AWS CLI, SDKs, or internal cloud service calls:

```text
API Call: "DeleteBucket" executed by IAM User "alice"
                           │
                           ▼
┌────────────────────────────────────────────────────────┐
│                    AWS CloudTrail                      │
│                                                        │
│  JSON Audit Log Record:                                │
│  • "userIdentity": "alice@company.com"                 │
│  • "eventTime": "2026-10-10T21:40:00Z"                 │
│  • "eventName": "DeleteBucket"                         │
│  • "sourceIPAddress": "198.51.100.4"                   │
│  • "responseElements": null                            │
└──────────────────────────┬─────────────────────────────┘
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
  Encrypted S3 Bucket (WORM)     CloudWatch Logs / Insights
```

---

## 📊 2. Management Events vs. Data Events

| Event Type | Scope & Nature | Enabled by Default? | Billing Model |
| :--- | :--- | :---: | :--- |
| **Management Events** | Control plane operations (e.g., `CreateSubnet`, `AttachRolePolicy`, `TerminateInstances`). | ✅ **Yes (Free 90-day history)** | 1st Multi-region copy is free. |
| **Data Events** | High-volume resource data plane actions (e.g., S3 `GetObject`, `PutObject`, Lambda `Invoke`). | ❌ Disabled by default | Charged per 100,000 events delivered. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - CloudWatch Logs Log Groups Log Streams and Insights](./02-CloudWatch-Logs-Log-Groups-Log-Streams-and-Insights.md) | [Index](../../../README.md) | [04 - AWS Config Compliance Rules and Remediation →](./04-AWS-Config-Compliance-Rules-and-Remediation.md) |
