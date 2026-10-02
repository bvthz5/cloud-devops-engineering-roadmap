# 01 - AWS Organizations & Multi-Account Strategy

Centralized management for multiple AWS accounts grouped into Organizational Units (OUs) (e.g. Workloads, Security, Sandbox).

```text
Management Account
 ├── Security OU ──> Security Tooling & Audit Accounts
 ├── Workloads OU ──> Prod & Non-Prod Accounts
 └── Sandbox OU ─────> Developer Sandbox Accounts
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (11-AWS-CloudWatch-and-CloudTrail)](../11-AWS-CloudWatch-and-CloudTrail/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Service Control Policies SCPs and Governance →](./02-Service-Control-Policies-SCPs-and-Governance.md) |
