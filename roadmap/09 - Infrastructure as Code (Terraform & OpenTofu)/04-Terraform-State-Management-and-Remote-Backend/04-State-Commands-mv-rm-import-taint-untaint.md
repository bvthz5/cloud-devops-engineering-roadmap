# 04 - State Commands: mv, rm, import, taint & untaint

## 1. State Manipulation Commands

### `terraform state mv` — Rename/Move Resources
```bash
# Rename a resource (no infrastructure change)
terraform state mv aws_instance.web aws_instance.api_server

# Move resource into a module
terraform state mv aws_instance.web module.compute.aws_instance.web

# Move between state files
terraform state mv -state-out=other.tfstate aws_instance.web aws_instance.web
```

### `terraform state rm` — Remove from State (Orphan)
```bash
# Remove resource from Terraform management (keeps cloud resource alive)
terraform state rm aws_instance.legacy_server
# Now Terraform won't track or destroy this resource
```

### `terraform import` — Import Existing Resources
```bash
# Import a pre-existing AWS VPC into Terraform state
terraform import aws_vpc.main vpc-0abc123def456

# Import with module path
terraform import module.networking.aws_vpc.main vpc-0abc123def456
```

### Import Block (Terraform ≥ 1.5 — Declarative)
```hcl
import {
  to = aws_instance.web
  id = "i-0abc123def456"
}

# Generate HCL configuration automatically
terraform plan -generate-config-out=generated.tf
```

## 2. `moved` Block (Terraform ≥ 1.1 — Refactoring)

```hcl
# Rename resource without state surgery
moved {
  from = aws_instance.web_server
  to   = aws_instance.api_gateway
}
# On next apply, Terraform recognizes the rename — no destroy/recreate
```

## 3. Replace (formerly taint/untaint)

```bash
# Force recreation of a resource on next apply
terraform apply -replace=aws_instance.web

# Old syntax (deprecated in 1.5+):
# terraform taint aws_instance.web
# terraform untaint aws_instance.web
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - State Locking Concurrency and Force Unlock](./03-State-Locking-Concurrency-and-Force-Unlock.md) | [Index](../../../README.md) | [05 - Sensitive Data in State and Encryption →](./05-Sensitive-Data-in-State-and-Encryption.md) |
