# 01 - HCL Language Fundamentals: Blocks, Arguments & Expressions

## 1. HCL Block Structure

Every `.tf` file is composed of **blocks** with a type, optional labels, and a body containing arguments.

```hcl
# Block syntax:  <BLOCK_TYPE> "<LABEL_1>" "<LABEL_2>" { ... }

resource "aws_instance" "web" {    # Block type: resource
  ami           = "ami-abc123"      # Argument
  instance_type = "t3.micro"        # Argument

  tags = {                          # Map argument
    Name = "web-server"
  }
}
```

## 2. Block Types in Terraform

| Block Type | Purpose | Example |
|---|---|---|
| `terraform` | Settings, backend, required providers | `terraform { required_version = ">= 1.5" }` |
| `provider` | Provider configuration | `provider "aws" { region = "us-east-1" }` |
| `resource` | Infrastructure resource definition | `resource "aws_s3_bucket" "data" { ... }` |
| `data` | Data source (read-only query) | `data "aws_ami" "latest" { ... }` |
| `variable` | Input variable declaration | `variable "region" { type = string }` |
| `output` | Output value declaration | `output "vpc_id" { value = aws_vpc.main.id }` |
| `locals` | Local computed values | `locals { env = "production" }` |
| `module` | Module invocation | `module "vpc" { source = "./modules/vpc" }` |
| `moved` | Resource refactoring | `moved { from = aws_instance.old to = aws_instance.new }` |
| `import` | Import existing resource | `import { to = aws_instance.web id = "i-123" }` |
| `check` | Post-apply assertions | `check "health" { ... }` |

## 3. Expressions

### String Interpolation
```hcl
name = "web-${var.environment}-${count.index}"
```

### Conditional Expression
```hcl
instance_type = var.environment == "prod" ? "t3.large" : "t3.micro"
```

### References
```hcl
# Resource attribute reference
subnet_id = aws_subnet.main.id

# Variable reference
region = var.region

# Local value reference
env_prefix = local.prefix

# Data source reference
ami = data.aws_ami.latest.id

# Module output reference
vpc_id = module.networking.vpc_id
```

## 4. Comments

```hcl
# Single-line comment (hash)

// Single-line comment (double slash)

/*
  Multi-line
  block comment
*/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (02-Terraform-Architecture-and-CLI)](../02-Terraform-Architecture-and-CLI/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Variables Types Validation and Precedence →](./02-Variables-Types-Validation-and-Precedence.md) |
