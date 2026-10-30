# 01 - HCL Language Fundamentals: Blocks, Arguments & Expressions

## 1. What Is HashiCorp Configuration Language (HCL2)?

**HashiCorp Configuration Language (HCL2)** is a declarative, human-readable domain-specific language (DSL) engineered specifically for infrastructure automation and cloud orchestration.

Before HCL, infrastructure tools forced developers into extreme trade-offs:
- **Raw JSON:** Difficult to read, lacks comments, no native variables or functions, and prone to syntax errors (trailing commas, escaped quotes).
- **YAML:** Sensitive to whitespace indentation errors, lacks strict type enforcement, and ambiguous string vs boolean parsing (`yes` / `no`).
- **General-Purpose Languages (Java, Python, C#):** Highly imperative, complex compilation steps, and lack native dependency graph compilation.

HCL strikes the optimal balance: it is as visually clear as YAML, as strictly typed and structured as JSON, but incorporates native support for **expressions, functions, multi-line heredocs, and graph references**.

```text
+---------------------------------------------------------------------------------------------------+
|                        HCL2: THE BRIDGE BETWEEN CONFIGURATION AND CODE                            |
+---------------------------------------------------------------------------------------------------+

 RAW JSON (VERBOSE & RIGID)      YAML (WHITESPACE FRAGILE)        HCL2 (CLEAN & EXPRESSIVE)
 {                               services:                        resource "aws_vpc" "main" {
   "resource": {                   vpc:                             cidr_block = "10.0.0.0/16"
     "aws_vpc": {                    cidr: 10.0.0.0/16              tags = {
       "main": {                       # Ambiguous typing!            Environment = "Production"
         "cidr": "10.0.0.0/16"                                      }
       }                                                          }
     }
   }
 }
```

---

## 2. Anatomy of an HCL Block

Every `.tf` and `.tofu` configuration file is composed of structural **Blocks**. A block has a **Type**, zero or more **Labels**, and a **Body** enclosed in curly braces `{}`:

$$\text{Syntax: } \texttt{<BLOCK\_TYPE> "<LABEL\_1>" "<LABEL\_2>" \{ <BODY> \}}$$

```hcl
# BLOCK_TYPE: resource
# LABEL_1:    aws_security_group (The resource type provided by AWS plugin)
# LABEL_2:    web_traffic (The internal logical name in HCL)
resource "aws_security_group" "web_traffic" {
  # 1. ARGUMENT: An identifier assigned a value using "="
  name        = "web-server-sg"
  description = "Permit HTTP and HTTPS ingress"
  vpc_id      = aws_vpc.main.id

  # 2. NESTED BLOCK: A block defined INSIDE a parent body (Notice: NO "=" sign!)
  ingress {
    description = "Allow TLS from everywhere"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # 3. MAP ARGUMENT: Key-value attributes assigned using "="
  tags = {
    Environment = "Production"
    Team        = "Platform"
  }
}
```

### The Critical Syntax Difference: Arguments vs. Nested Blocks
- **Argument (`name = "value"`):** Uses an equals sign `=`. Assigns a scalar, list, or map to a named attribute.
- **Nested Block (`ingress { ... }`):** Does **NOT** use an equals sign. Creates a repeated structural child block inside the resource.

---

## 3. The 11 Canonical Block Types in Modern Terraform / OpenTofu

Every statement in an IaC codebase belongs to one of eleven official block types:

| Block Type | Purpose | Primary Scope |
| :--- | :--- | :--- |
| **`terraform`** | Configures global settings, required CLI versions, and backend storage. | Global |
| **`provider`** | Configures credentials, regions, and settings for a cloud plugin (AWS, Azure).| Root / Aliases |
| **`resource`** | Declares a concrete cloud infrastructure component to create and manage. | Core Workload |
| **`data`** | Queries external information from the cloud without managing its lifecycle. | Read-Only |
| **`variable`** | Declares customizable input parameters passed into modules or roots. | Input Interface|
| **`output`** | Exports values to the terminal screen or to calling parent modules. | Output Interface|
| **`locals`** | Declares intermediate computed expressions to avoid code duplication. | Local Module |
| **`module`** | Instantiates a reusable child module from local disk or remote Git. | Composition |
| **`moved`** | Declarative state refactoring (renaming resources without recreation). | State Surgery |
| **`import`** | Declarative adoption of unmanaged cloud resources into state. | State Adoption |
| **`check`** | Post-apply continuous health validation assertions. | Quality Control |

---

## 4. Advanced Expressions and Comprehensions

HCL2 provides powerful functional programming expressions that eliminate imperative loops:

### 1. The Ternary Conditional Operator
```hcl
# Syntax: condition ? true_val : false_val
instance_type = var.environment == "production" ? "c6i.2xlarge" : "t3.micro"
```

### 2. List Comprehension (`for` expression returning a list)
Transforms an array of objects into a flat list of strings:
```hcl
variable "subnets" {
  type = list(object({ id = string, public = boolean }))
}

# Extracts IDs of only public subnets:
locals {
  public_ids = [for s in var.subnets : s.id if s.public == true]
}
```

### 3. Map Comprehension (`for` expression returning a map)
Transforms a list or map into a formatted lookup dictionary:
```hcl
variable "environments" {
  type    = list(string)
  default = ["dev", "stage", "prod"]
}

# Generates: { "dev" = "10.0.1.0/24", "stage" = "10.0.2.0/24", "prod" = "10.0.3.0/24" }
locals {
  cidr_allocations = {
    for idx, env in var.environments : env => "10.0.${idx + 1}.0/24"
  }
}
```

### 4. Splat Expressions (`[*]`)
A clean shorthand for extracting attributes from lists of resources:
```hcl
# Full for loop:
subnet_ids = [for s in aws_subnet.public : s.id]

# Identical result using Splat expression:
subnet_ids = aws_subnet.public[*].id
```

### 5. Multi-Line Strings: Heredocs (`<<-EOF`)
The indented heredoc syntax (`<<-`) automatically strips leading whitespace, allowing clean inline configuration templates without syntax errors:
```hcl
resource "aws_instance" "web" {
  user_data = <<-EOF
    #!/bin/bash
    echo "Starting Web Server..."
    yum update -y && yum install -y httpd
    systemctl start httpd
  EOF
}
```

---

## 5. Commenting Standards and Style Guide

HCL supports three distinct commenting styles:
- **`#` (Single-line comment - Recommended Standard):** Use for all general comments and explanations.
- **`//` (Single-line comment):** Supported for C/Go familiarity, but `#` is the official HashiCorp style recommendation.
- **`/* ... */` (Multi-line block comment):** Used for commenting out large blocks of code during debugging.

```hcl
# ==============================================================================
# NETWORKING TIER: HIGH AVAILABILITY VIRTUAL PRIVATE CLOUD (VPC)
# ==============================================================================
resource "aws_vpc" "production" {
  cidr_block           = "10.250.0.0/16"
  enable_dns_hostnames = true /* Enables AWS private DNS resolution */
  enable_dns_support   = true
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (02-Terraform-Architecture-and-CLI)](../02-Terraform-Architecture-and-CLI/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Variables Types Validation and Precedence →](./02-Variables-Types-Validation-and-Precedence.md) |
