# 02 - Service Control Policies (SCPs) & Multi-Account Governance

> **Focus:** AWS Organizations guardrails, SCP inheritance hierarchy, guardrail boundaries, and blocking unapproved AWS regions and services.

---

## 🛡️ 1. How Service Control Policies (SCPs) Work

A **Service Control Policy (SCP)** is an organization policy used to manage permissions in your organization. An SCP defines a **guardrail**, or limits, on the actions that account administrators and users can take:

```text
┌────────────────────────────────────────────────────────┐
│             Root Organization Unit (Root)              │
│       SCP: Restrict allowed regions to us-east-1       │
└───────────────────────────┬────────────────────────────┘
                            │ Inherited Downwards
                            ▼
┌────────────────────────────────────────────────────────┐
│            Production Organizational Unit (Prod OU)    │
│       SCP: Deny disabling AWS CloudTrail Logs          │
└───────────────────────────┬────────────────────────────┘
                            │ Inherited Downwards
                            ▼
┌────────────────────────────────────────────────────────┐
│              Production Workload Account               │
│  Even the ROOT ACCOUNT of this member account CANNOT   │
│  spin up VMs in Europe or disable CloudTrail!          │
└────────────────────────────────────────────────────────┘
```

> ⚠️ **Critical Architectural Rule:** **An SCP does NOT grant permissions.** An SCP only establishes the **maximum boundary** of allowable actions. The user must still be explicitly granted permissions via an IAM Policy to perform an action!

---

## 📜 2. Production SCP: Restrict Region Sprawl

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyAllOutsideAllowedRegions",
      "Effect": "Deny",
      "NotAction": [
        "iam:*",
        "route53:*",
        "cloudfront:*",
        "organizations:*",
        "support:*"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": [
            "us-east-1",
            "us-west-2"
          ]
        }
      }
    }
  ]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - AWS Organizations Multi Account Strategy and OUs](./01-AWS-Organizations-Multi-Account-Strategy-and-OUs.md) | [Index](../../../README.md) | [03 - AWS Control Tower and Landing Zone Architecture →](./03-AWS-Control-Tower-and-Landing-Zone-Architecture.md) |
