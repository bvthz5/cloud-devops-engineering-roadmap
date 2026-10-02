# 03 - Sentinel Policy Enforcement

## 1. Enforcement Levels

| Level | Behavior |
|---|---|
| **advisory** | Warns but allows apply to proceed |
| **soft-mandatory** | Blocks apply but can be overridden by admin |
| **hard-mandatory** | Blocks apply with no override possible |

## 2. Example Policy: No Public S3 Buckets

```hcl
import "tfplan/v2" as tfplan

no_public_s3 = rule {
  all tfplan.resource_changes as _, rc {
    rc.type is not "aws_s3_bucket_acl" or
    rc.change.after.acl is not "public-read"
  }
}

main = rule {
  no_public_s3
}
```

## 3. Policy Sets

```hcl
resource "tfe_policy_set" "security" {
  name          = "security-policies"
  organization  = "acme-corp"
  global        = true   # Apply to ALL workspaces

  vcs_repo {
    identifier     = "acme-corp/sentinel-policies"
    branch         = "main"
    oauth_token_id = tfe_oauth_client.github.oauth_token_id
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - VCS-Driven Runs](./02-VCS-Driven-Runs-and-Speculative-Plans.md) | [README](./README.md) | [04 - Private Registry](./04-Private-Registry-and-Run-Tasks.md) |
