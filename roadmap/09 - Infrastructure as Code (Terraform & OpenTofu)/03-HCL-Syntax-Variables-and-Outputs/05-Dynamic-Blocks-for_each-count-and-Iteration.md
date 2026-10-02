# 05 - Dynamic Blocks, for_each, count & Iteration

## 1. count vs for_each

| Aspect | `count` | `for_each` |
|---|---|---|
| **Index** | Numeric (`count.index`) | Key-based (`each.key`, `each.value`) |
| **Resource address** | `aws_instance.web[0]` | `aws_instance.web["prod"]` |
| **Reordering** | Removing item 0 shifts all indices → recreation | Keys are stable; only that key is removed |
| **Best for** | Simple numeric scaling | Maps, sets, named resources |

### count Example
```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-abc123"
  instance_type = "t3.micro"
  tags = { Name = "web-${count.index}" }
}
```

### for_each Example
```hcl
variable "environments" {
  default = {
    dev     = "t3.micro"
    staging = "t3.small"
    prod    = "t3.large"
  }
}

resource "aws_instance" "app" {
  for_each      = var.environments
  ami           = "ami-abc123"
  instance_type = each.value
  tags = { Name = "app-${each.key}" }
}
```

## 2. Dynamic Blocks

```hcl
variable "ingress_rules" {
  default = [
    { port = 80,  cidr = "0.0.0.0/0" },
    { port = 443, cidr = "0.0.0.0/0" },
    { port = 22,  cidr = "10.0.0.0/8" },
  ]
}

resource "aws_security_group" "web" {
  name = "web-sg"

  dynamic "ingress" {
    for_each = var.ingress_rules
    content {
      from_port   = ingress.value.port
      to_port     = ingress.value.port
      protocol    = "tcp"
      cidr_blocks = [ingress.value.cidr]
    }
  }
}
```

## 3. for Expressions

```hcl
# Transform a list
output "upper_names" {
  value = [for name in var.names : upper(name)]
}

# Filter a list
output "prod_instances" {
  value = [for k, v in aws_instance.app : v.id if k == "prod"]
}

# Transform to map
output "instance_ips" {
  value = { for k, v in aws_instance.app : k => v.public_ip }
}
```

## 4. Conditional Resource Creation

```hcl
resource "aws_cloudwatch_log_group" "app" {
  count = var.enable_logging ? 1 : 0
  name  = "/app/logs"
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Locals Expressions and Built in Functions](./04-Locals-Expressions-and-Built-in-Functions.md) | [Index](../../../README.md) | [06 - Type Constraints Complex Types and any →](./06-Type-Constraints-Complex-Types-and-any.md) |
