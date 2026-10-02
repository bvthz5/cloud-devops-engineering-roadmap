# 03 - removed Block & State Surgery

## 1. removed Block (>= 1.7)

```hcl
removed {
  from = aws_instance.legacy_server

  lifecycle {
    destroy = false   # Keep the cloud resource, just stop managing it
  }
}
```

## 2. State Surgery Commands

```bash
# Remove from state (cloud resource stays)
terraform state rm aws_instance.legacy

# Move resource between state files
terraform state mv -state-out=other.tfstate aws_instance.web aws_instance.web

# List all resources
terraform state list

# Show resource details
terraform state show aws_instance.web
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - moved Block Refactoring](./02-moved-Block-Refactoring.md) | [Index](../../../README.md) | [04 - Generate Config from Import →](./04-Generate-Config-from-Import.md) |
