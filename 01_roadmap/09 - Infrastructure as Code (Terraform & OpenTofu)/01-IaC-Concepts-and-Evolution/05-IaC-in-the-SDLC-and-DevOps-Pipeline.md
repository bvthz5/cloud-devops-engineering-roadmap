# 05 - IaC in the SDLC & DevOps Pipeline

## 1. IaC as Part of the Software Development Lifecycle

```text
┌─────────────────────────────────────────────────────────────────┐
│              IaC IN THE DEVELOPMENT WORKFLOW                     │
│                                                                  │
│  Developer writes .tf ──► Git push ──► PR created               │
│       │                                    │                     │
│       ▼                                    ▼                     │
│  Local: terraform plan            CI: terraform plan             │
│  (developer preview)              (automated preview in PR)      │
│                                    │                             │
│                                    ▼                             │
│                              Peer Review                         │
│                              (Team reviews plan output)          │
│                                    │                             │
│                                    ▼                             │
│                              Merge to main                       │
│                                    │                             │
│                                    ▼                             │
│                              CD: terraform apply                 │
│                              (automated deployment)              │
└─────────────────────────────────────────────────────────────────┘
```

## 2. Blast Radius Control Strategies

| Strategy | Description |
|---|---|
| **Small State Files** | Split infrastructure into multiple Terraform root modules (networking, compute, database) instead of one monolith |
| **Environment Isolation** | Separate state per environment (dev/staging/prod) with different cloud accounts |
| **Resource Targeting** | Use `terraform apply -target=aws_instance.web` for emergency single-resource changes |
| **Plan Review Gates** | Require human approval before `apply` in production pipelines |
| **Sentinel / OPA Policies** | Policy-as-code guardrails that prevent dangerous changes (e.g., "no public S3 buckets") |
| **Terraform Cloud Run Tasks** | Pre-plan and post-plan hooks for cost estimation, security scanning, and compliance |

## 3. GitOps for Infrastructure

```text
Git Repository (Single Source of Truth)
    │
    ├── infrastructure/
    │   ├── networking/          ← VPC, subnets, NAT gateways
    │   ├── compute/             ← EC2, ASGs, launch templates
    │   ├── database/            ← RDS, ElastiCache, DynamoDB
    │   ├── security/            ← IAM roles, KMS keys, SGs
    │   └── monitoring/          ← CloudWatch, SNS, dashboards
    │
    └── .github/workflows/
        └── terraform.yml        ← CI/CD pipeline
```

## 4. Progressive Rollout Pattern

```text
Feature Branch ──► Dev Apply (auto) ──► Staging Apply (auto) ──► Prod Apply (manual gate)
                       │                      │                        │
                       ▼                      ▼                        ▼
                  Dev Account            Staging Account          Prod Account
                  (destroy OK)           (soak test 24h)          (change window)
```

## 5. IaC Code Review Checklist

- [ ] Does `terraform plan` show only expected changes?
- [ ] Are there any unexpected `destroy` or `replace` operations?
- [ ] Are sensitive values marked as `sensitive = true`?
- [ ] Are resource names following team naming conventions?
- [ ] Is the blast radius acceptable (how many resources affected)?
- [ ] Are cost implications reviewed (use Infracost)?
- [ ] Do security policies pass (tfsec, Checkov, Sentinel)?

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - IaC Workflow Write Plan Apply](./04-IaC-Workflow-Write-Plan-Apply.md) | [Index](../../../README.md) | [06 - OpenTofu The Open Source Fork →](./06-OpenTofu-The-Open-Source-Fork.md) |
