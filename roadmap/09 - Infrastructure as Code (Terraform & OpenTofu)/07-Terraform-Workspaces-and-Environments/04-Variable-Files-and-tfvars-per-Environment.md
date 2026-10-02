# 04 - Variable Files & tfvars per Environment

## 1. tfvars File Strategy

```text
environments/
+-- common.tfvars        <- Shared across all environments
+-- dev.tfvars           <- Dev-specific overrides
+-- staging.tfvars       <- Staging-specific overrides
+-- prod.tfvars          <- Prod-specific overrides
```

```bash
# Apply with environment-specific variables
terraform apply -var-file=common.tfvars -var-file=prod.tfvars
```

## 2. Example tfvars Files

```hcl
# common.tfvars
project_name = "acme-api"
team         = "platform"

# dev.tfvars
environment    = "dev"
instance_type  = "t3.micro"
instance_count = 1
enable_monitoring = false

# prod.tfvars
environment    = "prod"
instance_type  = "t3.large"
instance_count = 3
enable_monitoring = true
```

## 3. Auto-Loaded Variable Files

Terraform automatically loads these files (no `-var-file` flag needed):
- `terraform.tfvars`
- `*.auto.tfvars` (alphabetical order)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Environment Segregation Patterns](./03-Environment-Segregation-Patterns.md) | [Index](../../../README.md) | [05 - Terraform Cloud Workspaces and VCS Workflows →](./05-Terraform-Cloud-Workspaces-and-VCS-Workflows.md) |
