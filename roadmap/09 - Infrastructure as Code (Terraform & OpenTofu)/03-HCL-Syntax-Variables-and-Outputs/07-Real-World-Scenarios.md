# 07 - Real-World Scenarios

## Scenario 01: Dynamic Security Group Rules at Scale

### Problem
An enterprise needed to manage 200+ ingress rules across 15 security groups. Hardcoding each rule block made files 3,000+ lines long and unmaintainable.

### Solution
Used `dynamic` blocks with `for_each` over a YAML-defined rules map:
```hcl
locals {
  sg_rules = yamldecode(file("${path.module}/sg-rules.yaml"))
}

resource "aws_security_group" "app" {
  for_each = local.sg_rules

  dynamic "ingress" {
    for_each = each.value.ingress
    content {
      from_port   = ingress.value.port
      to_port     = ingress.value.port
      protocol    = "tcp"
      cidr_blocks = ingress.value.cidrs
    }
  }
}
```
Result: 3,000 lines → 40 lines of HCL + 1 YAML file.

---

## Scenario 02: count Index Reordering Disaster

### Problem
A team used `count = 5` for database instances. When they removed instance `[2]`, instances `[3]` and `[4]` were renumbered to `[2]` and `[3]`, causing Terraform to destroy and recreate 2 production databases.

### Solution
Migrated to `for_each` with stable string keys (`"db-primary"`, `"db-replica-1"`, etc.) so removing any single key only affects that specific resource.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Type Constraints Complex Types and any](./06-Type-Constraints-Complex-Types-and-any.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
