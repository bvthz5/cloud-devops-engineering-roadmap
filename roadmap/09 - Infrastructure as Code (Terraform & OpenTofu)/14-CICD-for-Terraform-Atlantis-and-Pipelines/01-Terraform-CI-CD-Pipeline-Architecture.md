# 01 - Terraform CI/CD Pipeline Architecture

```text
Feature Branch              Main Branch
    |                           |
    v                           v
PR Created                  Merge
    |                           |
    v                           v
terraform fmt -check        terraform plan -out=tfplan
terraform validate              |
tflint                          v
tfsec / checkov             terraform apply tfplan
terraform plan                  |
    |                           v
    v                       Notify (Slack/Teams)
Post plan as PR comment
```

## Key Principles

| Principle | Description |
|---|---|
| Plan on PR | Every PR shows infrastructure diff as a comment |
| Apply on merge | Only merged code gets applied |
| Saved plans | `plan -out=tfplan` then `apply tfplan` for consistency |
| Approval gates | Production requires manual approval |
| Ephemeral credentials | Use OIDC, not long-lived access keys |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - GitHub Actions](./02-GitHub-Actions-for-Terraform.md) |
