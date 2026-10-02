# 03 - State Architecture at Scale

## State Splitting Strategy

```text
Organization
+-- Account: shared-services
|   +-- state: networking (VPC, Transit Gateway)
|   +-- state: security (IAM, KMS)
+-- Account: dev
|   +-- state: compute (ECS, EC2)
|   +-- state: data (RDS, DynamoDB)
+-- Account: prod
    +-- state: compute
    +-- state: data
    +-- state: monitoring
```

## Cross-State References

Use `terraform_remote_state` data source sparingly. Prefer outputs published to SSM Parameter Store or a service discovery mechanism to reduce coupling.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Multi-Team Patterns](./02-Multi-Team-Collaboration-Patterns.md) | [README](./README.md) | [04 - RBAC & Governance](./04-RBAC-and-Governance-for-IaC.md) |
