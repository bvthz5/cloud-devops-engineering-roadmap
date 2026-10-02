# 02 - Provider Registry, Installation & Version Pinning

## 1. Terraform Registry Architecture

```text
registry.terraform.io
    ├── Providers (3,500+)
    │   ├── hashicorp/aws         (Official)
    │   ├── hashicorp/azurerm     (Official)
    │   ├── hashicorp/google      (Official)
    │   ├── integrations/github   (Partner)
    │   └── community/custom      (Community)
    │
    └── Modules (15,000+)
        ├── terraform-aws-modules/vpc
        ├── terraform-aws-modules/eks
        └── terraform-google-modules/network
```

## 2. Version Constraint Operators

| Operator | Example | Meaning |
|---|---|---|
| `=` | `= 5.30.0` | Exactly this version |
| `!=` | `!= 5.30.0` | Any version except this |
| `>`, `>=`, `<`, `<=` | `>= 5.0, < 6.0` | Range constraints |
| `~>` | `~> 5.30` | Pessimistic: allows `5.30.x` but not `5.31.0` |
| `~>` | `~> 5.0` | Allows `5.x.x` but not `6.0.0` |

## 3. Dependency Lock File (`.terraform.lock.hcl`)

```hcl
# This file is maintained automatically by "terraform init".
provider "registry.terraform.io/hashicorp/aws" {
  version     = "5.31.0"
  constraints = "~> 5.0"
  hashes = [
    "h1:abc123...",
    "zh:def456...",
  ]
}
```

**Rules:**
- ✅ **Always commit** `.terraform.lock.hcl` to Git
- ❌ **Never commit** `.terraform/` directory
- Run `terraform init -upgrade` to update provider versions within constraints

## 4. Provider Mirrors (Air-Gapped / Corporate)

```bash
# Create a filesystem mirror
terraform providers mirror /path/to/mirror

# Configure in .terraformrc (CLI config)
provider_installation {
  filesystem_mirror {
    path    = "/path/to/mirror"
    include = ["registry.terraform.io/hashicorp/*"]
  }
  direct {
    exclude = ["registry.terraform.io/hashicorp/*"]
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Core Architecture](./01-Terraform-Core-Architecture-Providers-and-Plugin-Protocol.md) | [README](./README.md) | [03 - CLI Deep Dive](./03-Terraform-CLI-Deep-Dive-Init-Plan-Apply-Destroy.md) |
