# 06 - State Splitting & Multi-State Architecture

## 1. Why Split State?

| Monolithic State | Split State |
|---|---|
| 2000+ resources in one state | 50-200 resources per state |
| `terraform plan` takes 15+ minutes | 30-90 seconds per module |
| Single lock blocks all teams | Each team has independent locks |
| One mistake can destroy everything | Blast radius limited to one domain |

## 2. Recommended Split Pattern

```text
terraform-infrastructure/
├── networking/          ← VPC, subnets, NAT, transit gateway
│   ├── main.tf
│   └── outputs.tf      (exports vpc_id, subnet_ids)
├── security/            ← IAM roles, KMS keys, security groups
│   ├── main.tf
│   └── outputs.tf
├── data/                ← RDS, ElastiCache, DynamoDB
│   ├── main.tf
│   └── outputs.tf
├── compute/             ← EC2, ASGs, ECS, EKS
│   ├── main.tf
│   └── outputs.tf
└── monitoring/          ← CloudWatch, SNS, PagerDuty
    ├── main.tf
    └── outputs.tf
```

## 3. Cross-State Dependencies

```text
networking (state 1)  ──► outputs: vpc_id, subnet_ids
      │
      ▼
compute (state 2) ──► reads: terraform_remote_state.networking
      │
      ▼
monitoring (state 3) ──► reads: terraform_remote_state.compute
```

## 4. Dependency Order for `apply`

```bash
# Must apply in dependency order
cd networking  && terraform apply
cd security    && terraform apply
cd data        && terraform apply
cd compute     && terraform apply
cd monitoring  && terraform apply
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Sensitive Data in State and Encryption](./05-Sensitive-Data-in-State-and-Encryption.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
