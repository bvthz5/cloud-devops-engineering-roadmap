# 05 - IAM Policy Evaluation Logic & The Explicit Deny Rule

> **Focus:** AWS authorization decision engine, default deny, explicit allow, explicit deny precedence, SCP boundaries, and policy evaluation flowcharts.

---

## 🚦 1. The Golden Rule of IAM Policy Evaluation

```text
Default State: DENY
      │
      ▼
Is there an EXPLICIT DENY? ─────YES────► 🛑 Access DENIED (Overrides Everything!)
      │
      NO
      ▼
Is there an EXPLICIT ALLOW? ────YES────► ✅ Access GRANTED
      │
      NO
      ▼
🛑 Access DENIED (Implicit Default Deny)
```

> ⚠️ **The Golden Security Rule:** **An Explicit Deny ALWAYS trumps an Explicit Allow.** If a user has 10 policies granting full administrator access, but 1 policy states `Deny` on `s3:DeleteObject`, that user will **never** be able to delete an S3 object.

---

## 📊 2. Multi-Layer Policy Evaluation Order

In an enterprise AWS organization, authorization requests pass through up to 6 distinct gates:

```text
1. AWS Organizations Service Control Policy (SCP) ➔ Must ALLOW
                        │
2. Resource-Based Policy (e.g., S3 Bucket Policy) ➔ Must ALLOW / Not Denied
                        │
3. IAM Permissions Boundary ➔ Must ALLOW
                        │
4. Session Policy (STS AssumeRole token) ➔ Must ALLOW
                        │
5. Identity-Based Policy (User / Role attached policy) ➔ Must ALLOW
                        │
                   FINAL DECISION
```

---

## 📜 3. Enterprise Policy: Enforcing MFA for Sensitive Operations

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "BlockAllWithoutMFA",
      "Effect": "Deny",
      "NotAction": [
        "iam:CreateVirtualMFADevice",
        "iam:EnableMFADevice",
        "iam:GetUser",
        "iam:ListMFADevices",
        "iam:ResyncMFADevice"
      ],
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"
        }
      }
    }
  ]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - IAM Policies JSON Structure Identity vs Resource](./04-IAM-Policies-JSON-Structure-Identity-vs-Resource.md) | [Index](../../../README.md) | [06 - AWS IAM Identity Center Single Sign On SSO →](./06-AWS-IAM-Identity-Center-Single-Sign-On-SSO.md) |
