# 06 - Cost Estimation & Audit Logging

## 1. Built-in Cost Estimation

Terraform Cloud estimates monthly costs for AWS, Azure, and GCP resources during every plan.

```text
Plan output:
  + aws_instance.web (t3.large)
    Monthly cost: $60.74
  + aws_rds_instance.db (db.r6g.large)
    Monthly cost: $175.20

Total monthly cost: $235.94
```

## 2. Audit Logging

All actions in Terraform Cloud are logged:
- Who triggered a run
- What changes were planned
- Who approved the apply
- Policy check results
- State access events

## 3. RBAC (Role-Based Access Control)

| Role | Permissions |
|---|---|
| **Read** | View workspace, state, runs |
| **Plan** | Queue plans, read state |
| **Write** | Queue plans, approve applies |
| **Admin** | Full workspace management |
| **Owner** | Organization-level admin |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Agent Pools and Self Hosted Runners](./05-Agent-Pools-and-Self-Hosted-Runners.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
