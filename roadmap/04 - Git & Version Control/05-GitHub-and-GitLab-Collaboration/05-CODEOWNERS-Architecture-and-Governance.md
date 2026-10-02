# 05 - CODEOWNERS Architecture and Governance

## 1. What is CODEOWNERS?

The `CODEOWNERS` file automatically assigns reviewers and enforces required sign-offs whenever PRs touch specific paths or files.
- Location: `.github/CODEOWNERS` or `.gitlab/CODEOWNERS`

---

## 2. Production CODEOWNERS Syntax Example

```text
# Default fallback: Global DevOps SRE team owns everything
* @company/core-devops

# Security team must approve any changes to authentication or secrets
/src/auth/          @company/security-team
*.key               @company/security-team

# Infrastructure team owns Terraform and Kubernetes manifests
/terraform/         @company/cloud-platform
/k8s/               @company/cloud-platform

# Data team owns database migrations
/migrations/        @company/dba-team
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Branch Protection Rules and Merge Queues](./04-Branch-Protection-Rules-and-Merge-Queues.md) | [Index](../../../README.md) | [06 - GitHub Releases and Release Automation →](./06-GitHub-Releases-and-Release-Automation.md) |
