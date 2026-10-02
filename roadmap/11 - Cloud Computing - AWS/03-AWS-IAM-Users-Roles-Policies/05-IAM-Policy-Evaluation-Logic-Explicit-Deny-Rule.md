# 05 - IAM Policy Evaluation Logic

```text
 1. Is there an EXPLICIT DENY? ──> YES ──> DENY ACCESS [STOP]
            │ NO
            ▼
 2. Is there an EXPLICIT ALLOW? ──> YES ──> ALLOW ACCESS [STOP]
            │ NO
            ▼
 3. IMPLICIT DENY ───────────────> DENY ACCESS [DEFAULT]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - IAM Policies JSON Structure Identity vs Resource](./04-IAM-Policies-JSON-Structure-Identity-vs-Resource.md) | [Index](../../../README.md) | [06 - AWS IAM Identity Center Single Sign On SSO →](./06-AWS-IAM-Identity-Center-Single-Sign-On-SSO.md) |
