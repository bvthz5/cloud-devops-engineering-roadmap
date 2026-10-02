# 05 - Drift Detection Strategies & Automation

## 1. What Is Drift?

Drift occurs when real infrastructure diverges from IaC-defined state due to manual changes, other automation, or external events.

## 2. Detection Methods

| Method | Tool | Frequency |
|---|---|---|
| Scheduled `terraform plan` | CI cron job | Every 4-12 hours |
| Cloud provider config rules | AWS Config, Azure Policy | Real-time |
| Terraform Cloud drift detection | HCP Terraform | Configurable |
| Custom scripts | AWS SDK + diff | On-demand |

## 3. Automated Drift Detection Pipeline

```yaml
# .github/workflows/drift-check.yml
name: Drift Detection
on:
  schedule:
    - cron: '0 */6 * * *'   # Every 6 hours

jobs:
  drift:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Terraform Plan
        run: |
          terraform init
          terraform plan -detailed-exitcode
          # Exit code 0 = no changes (no drift)
          # Exit code 2 = changes detected (drift!)
      - name: Alert on Drift
        if: failure()
        run: |
          curl -X POST "$SLACK_WEBHOOK" \
            -d '{"text":"DRIFT DETECTED in production infrastructure!"}'
```

## 4. `plan -detailed-exitcode`

| Exit Code | Meaning |
|---|---|
| 0 | No changes (infrastructure matches IaC) |
| 1 | Error occurred |
| 2 | Changes detected (drift present) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Integration Testing Terratest and Kitchen Terraform](./04-Integration-Testing-Terratest-and-Kitchen-Terraform.md) | [Index](../../../README.md) | [06 - Policy as Code Sentinel OPA and Conftest →](./06-Policy-as-Code-Sentinel-OPA-and-Conftest.md) |
