# 07 - Real-World Scenarios & Post-Mortems

## Scenario 01: The Configuration Drift Disaster

### Incident Summary
A mid-size fintech company managed 200+ AWS resources with Terraform but allowed developers direct AWS Console access "for debugging." Over 6 months, 47 resources drifted from their Terraform-defined state.

### What Happened
1. A developer manually changed an S3 bucket policy to allow public access for a demo.
2. The change was never reflected in `.tf` files.
3. Three months later, a `terraform apply` reverted the bucket policy, breaking a production integration.
4. Simultaneously, it destroyed a manually created Lambda function that had become business-critical.

### Resolution
1. Implemented `terraform import` for all manually created resources.
2. Enforced **no console access** — all changes through IaC PRs only.
3. Set up weekly `terraform plan` drift detection alerts.
4. Deployed AWS Config rules to detect non-IaC-managed resources.

---

## Scenario 02: Monolithic State File Explosion

### Incident Summary
A startup managed ALL infrastructure (networking, compute, databases, monitoring, IAM) in a single Terraform state file with 2,300 resources.

### Problems
1. `terraform plan` took 18 minutes due to API rate limiting.
2. State file locking blocked all teams from deploying simultaneously.
3. A single typo in a network variable caused a cascading `destroy` of 400 resources.

### Resolution
1. Split into domain-specific state files: `networking`, `compute`, `data`, `security`, `monitoring`.
2. Used `terraform_remote_state` data source for cross-state references.
3. Reduced plan time from 18 minutes to 90 seconds per module.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - OpenTofu The Open Source Fork](./06-OpenTofu-The-Open-Source-Fork.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
