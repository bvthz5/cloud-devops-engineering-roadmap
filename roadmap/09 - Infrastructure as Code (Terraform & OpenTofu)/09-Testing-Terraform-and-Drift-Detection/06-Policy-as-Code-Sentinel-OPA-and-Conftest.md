# 06 - Policy as Code: Sentinel, OPA & Conftest

## 1. Tool Comparison

| Tool | Language | Integration | License |
|---|---|---|---|
| **Sentinel** | Sentinel DSL | Terraform Cloud only | Proprietary |
| **OPA / Rego** | Rego | Any CI/CD pipeline | Apache 2.0 |
| **Conftest** | Rego (OPA) | CLI, GitHub Actions | Apache 2.0 |
| **Checkov** | Python (YAML rules) | CLI, CI/CD | Apache 2.0 |

## 2. OPA/Conftest Example

```rego
# policy/deny_public_s3.rego
package main

deny[msg] {
  resource := input.resource_changes[_]
  resource.type == "aws_s3_bucket"
  resource.change.after.acl == "public-read"
  msg := sprintf("S3 bucket '%s' must not be public", [resource.address])
}
```

```bash
terraform plan -out=tfplan
terraform show -json tfplan > plan.json
conftest test plan.json --policy policy/
```

## 3. Sentinel Example (Terraform Cloud)

```hcl
# Sentinel policy
import "tfplan/v2" as tfplan

main = rule {
  all tfplan.resource_changes as _, rc {
    rc.type is not "aws_s3_bucket" or
    rc.change.after.acl is not "public-read"
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Drift Detection Strategies and Automation](./05-Drift-Detection-Strategies-and-Automation.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
