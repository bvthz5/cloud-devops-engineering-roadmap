# 04 - Module Design Patterns & Best Practices

## 1. Module Design Patterns

### Pattern 1: Thin Wrapper Module
```text
Wraps a single resource with sensible defaults and validation.
Small scope, highly reusable.
```

### Pattern 2: Opinionated Module
```text
Bundles multiple resources into a complete solution (VPC + subnets + NAT + IGW).
Enforces organizational standards via hardcoded best practices.
```

### Pattern 3: Composable Module
```text
Small, focused modules composed together in root modules.
VPC module + Subnet module + NAT module (each independent).
```

## 2. Naming Conventions

```text
terraform-<PROVIDER>-<NAME>
Examples:
  terraform-aws-vpc
  terraform-google-gke
  terraform-azurerm-network
```

## 3. Anti-Patterns to Avoid

| Anti-Pattern | Problem | Solution |
|---|---|---|
| God module | One module manages everything | Split by domain |
| Hardcoded values | No flexibility | Use variables with defaults |
| No version pinning | Breaking changes | Pin versions with `~>` |
| Nested modules > 3 levels | Complexity explosion | Flatten module hierarchy |
| Using `count` in modules | Index shifting | Use `for_each` |

## 4. Module Documentation Standard

Every module should include:
- `README.md` with description, inputs table, outputs table, and usage examples
- `variables.tf` with `description` on every variable
- `outputs.tf` with `description` on every output
- `CHANGELOG.md` with semantic versioning

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Module Sources](./03-Module-Sources-Registry-Git-S3-and-Local.md) | [README](./README.md) | [05 - Publishing Modules](./05-Publishing-Modules-to-Terraform-Registry.md) |
