# 05 - Terraform Cloud Workspaces & VCS Workflows

## 1. Terraform Cloud vs CLI Workspaces

| Feature | CLI Workspaces | Terraform Cloud Workspaces |
|---|---|---|
| State storage | Local/S3/GCS | Terraform Cloud (managed) |
| Run environment | Local machine | Terraform Cloud runners |
| VCS integration | None | GitHub/GitLab/Bitbucket |
| Policy enforcement | None (manual) | Sentinel / OPA policies |
| Cost estimation | None | Built-in Infracost |
| RBAC | None | Team-based permissions |
| Audit logging | None | Full audit trail |

## 2. VCS-Driven Workflow

```text
Developer pushes to feature branch
        |
        v
Terraform Cloud detects change
        |
        v
Automatic "speculative plan" on PR
        |
        v
Team reviews plan output in PR
        |
        v
Merge to main
        |
        v
Terraform Cloud runs "apply" automatically
```

## 3. Alternative Platforms

| Platform | Open Source | Key Differentiator |
|---|---|---|
| Terraform Cloud / HCP | No (freemium) | Official HashiCorp product |
| Spacelift | No | Advanced policy, drift detection |
| env0 | No | Self-service environments |
| Scalr | No | OPA policies, hierarchical config |
| Atlantis | Yes | GitHub PR-based workflow (self-hosted) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Variable Files](./04-Variable-Files-and-tfvars-per-Environment.md) | [README](./README.md) | [06 - Multi-Account Deployment](./06-Multi-Account-Multi-Region-Deployment.md) |
