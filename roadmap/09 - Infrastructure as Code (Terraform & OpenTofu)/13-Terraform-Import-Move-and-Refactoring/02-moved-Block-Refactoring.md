# 02 - moved Block Refactoring

## 1. Rename a Resource

```hcl
moved {
  from = aws_instance.web_server
  to   = aws_instance.api_gateway
}
```

On next `terraform apply`, Terraform recognizes this as a rename, not a destroy/recreate.

## 2. Move into a Module

```hcl
moved {
  from = aws_vpc.main
  to   = module.networking.aws_vpc.main
}
```

## 3. Move between Modules

```hcl
moved {
  from = module.old_network.aws_vpc.main
  to   = module.new_network.aws_vpc.primary
}
```

**Best Practice:** Keep `moved` blocks for at least one release cycle so all team members run apply with the rename. Then remove them.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - terraform import CLI and Import Block](./01-terraform-import-CLI-and-Import-Block.md) | [Index](../../../README.md) | [03 - removed Block and State Surgery →](./03-removed-Block-and-State-Surgery.md) |
