# 02 - VCS-Driven Runs & Speculative Plans

## 1. VCS-Driven Workflow

```text
Developer pushes to branch
    |
    v
PR opened --> Speculative plan (read-only, shown in PR)
    |
    v
Reviewer approves PR + plan output
    |
    v
Merge to main --> Real plan + apply (with approval for prod)
```

## 2. Workspace Configuration

```hcl
# Using the tfe provider to configure workspaces programmatically
resource "tfe_workspace" "api" {
  name              = "api-production"
  organization      = "acme-corp"
  terraform_version = "1.7.0"

  vcs_repo {
    identifier     = "acme-corp/infrastructure"
    branch         = "main"
    oauth_token_id = tfe_oauth_client.github.oauth_token_id
  }

  working_directory   = "services/api"
  auto_apply          = false           # Require manual approval
  speculative_enabled = true            # Enable speculative plans on PRs
}
```

## 3. Run Triggers (Cross-Workspace Dependencies)

```text
Workspace: networking (apply)
    |
    v (triggers)
Workspace: compute (auto-plan)
    |
    v (triggers)
Workspace: monitoring (auto-plan)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Terraform Cloud Architecture and Pricing Tiers](./01-Terraform-Cloud-Architecture-and-Pricing-Tiers.md) | [Index](../../../README.md) | [03 - Sentinel Policy Enforcement →](./03-Sentinel-Policy-Enforcement.md) |
