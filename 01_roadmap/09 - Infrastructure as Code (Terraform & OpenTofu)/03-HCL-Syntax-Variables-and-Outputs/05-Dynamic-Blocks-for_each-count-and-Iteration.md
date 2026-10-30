# 05 - Dynamic Blocks, for_each, count & Iteration

## 1. The "Count Reindexing Disaster" (Why `count` Is Dangerous)

In early versions of Terraform, the only mechanism available to create multiple copies of a resource was the **`count`** meta-argument.

While `count` is safe for simple identical scaling (e.g., `count = 3` identical worker nodes), **using `count` over a list of items is one of the most dangerous anti-patterns in Infrastructure as Code**.

### Step-by-Step Breakdown of the Catastrophe:
Suppose you define three subnets in different availability zones using `count` over a list:

```hcl
variable "zones" {
  default = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

resource "aws_subnet" "tiers" {
  count             = length(var.zones)
  availability_zone = var.zones[count.index]
  cidr_block        = "10.0.${count.index}.0/24"
}
```

In the state file, Terraform indexes these subnets by integer position:
- `aws_subnet.tiers[0]` $\to$ `us-east-1a`
- `aws_subnet.tiers[1]` $\to$ `us-east-1b`
- `aws_subnet.tiers[2]` $\to$ `us-east-1c`

Now suppose you remove `"us-east-1a"` from the list: `["us-east-1b", "us-east-1c"]`.
- `us-east-1b` shifts from index `[1]` to index `[0]`.
- `us-east-1c` shifts from index `[2]` to index `[1]`.
- Index `[2]` no longer exists.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE DESTRUCTIVE COUNT INDEX SHIFTING CATASTROPHE                           |
+---------------------------------------------------------------------------------------------------+

 BEFORE REMOVING ITEM 0:
 Index [0] = us-east-1a (Active Production Subnet)
 Index [1] = us-east-1b (Active Production Subnet)
 Index [2] = us-east-1c (Active Production Subnet)

 YOU REMOVE "us-east-1a" FROM LIST ──► ARRAY INDICES SHIFT!
 
 TERRAFORM PLAN INTERPRETATION:
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ -/+: Index [0] changed AZ from "us-east-1a" to "us-east-1b" ──► DESTROY & RECREATE│
 │ -/+: Index [1] changed AZ from "us-east-1b" to "us-east-1c" ──► DESTROY & RECREATE│
 │   -: Index [2] was deleted ──► DESTROY!                                          │
 └──────────────────────────────────────────────────────────────────────────────────┘
 RESULT: TERRAFORM DESTROYS ALL THREE PRODUCTION SUBNETS SIMULTANEOUSLY!
```

---

## 2. The Solution: `for_each` and Stable Map Keys

To permanently eliminate index shifting, Terraform 0.12.6 introduced **`for_each`**.

Instead of numeric array indices (`[0]`, `[1]`), `for_each` binds resources to **stable, immutable map keys or string set elements**:

```hcl
variable "zones" {
  type    = set(string)
  default = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

resource "aws_subnet" "tiers" {
  for_each          = var.zones
  availability_zone = each.key
  cidr_block        = each.key == "us-east-1a" ? "10.0.1.0/24" : "10.0.2.0/24"
}
```

In the state file, resources are addressed by key:
- `aws_subnet.tiers["us-east-1a"]`
- `aws_subnet.tiers["us-east-1b"]`
- `aws_subnet.tiers["us-east-1c"]`

If you delete `"us-east-1a"` from the set, Terraform executes a single surgical deletion:
```text
Plan: 0 to add, 0 to change, 1 to destroy.
  - aws_subnet.tiers["us-east-1a"] will be destroyed
```
The remaining subnets (`"us-east-1b"` and `"us-east-1c"`) are completely unaffected!

---

## 3. Comprehensive Comparison: `count` vs. `for_each`

| Evaluation Criteria | `count` | `for_each` |
| :--- | :--- | :--- |
| **Addressing Scheme** | Integer index: `resource.name[0]` | String key: `resource.name["key"]` |
| **Input Data Type** | Numbers or `length(list)` | `map` or `set(string)` (use `toset(list)`) |
| **Array Removal Behavior** | **Disastrous:** Shifts all indices, causing fleet-wide recreation. | **Safe:** Destroys strictly the deleted key; leaves others untouched. |
| **Conditional Toggle** | Ideal for 0 or 1 toggle: `count = var.enabled ? 1 : 0` | Verbose for simple boolean toggles. |
| **Best Used For** | Identical numeric scaling or boolean enablement. | **All heterogeneous or named resources (Subnets, Users, Buckets).** |

---

## 4. Dynamic Blocks: Programmatic Nested Configuration

Many cloud resources require repeating **nested structural blocks** within their body. For example, an `aws_security_group` can contain multiple `ingress {}` blocks, or an `aws_elastic_beanstalk_environment` contains multiple `setting {}` blocks.

Writing these nested blocks manually leads to messy copy-pasting. **Dynamic Blocks** allow you to iterate over a collection and generate nested blocks programmatically:

```hcl
variable "firewall_rules" {
  type = list(object({
    port        = number
    description = string
    cidr        = string
  }))
  default = [
    { port = 80,   description = "HTTP Traffic",  cidr = "0.0.0.0/0" },
    { port = 443,  description = "HTTPS Traffic", cidr = "0.0.0.0/0" },
    { port = 8443, description = "Admin Console", cidr = "10.0.0.0/8" },
  ]
}

resource "aws_security_group" "application_firewall" {
  name        = "app-firewall-sg"
  description = "Dynamically generated enterprise firewall rules"
  vpc_id      = aws_vpc.main.id

  # DYNAMIC BLOCK SYNTAX:
  # dynamic "<NESTED_BLOCK_NAME>" { ... }
  dynamic "ingress" {
    for_each = var.firewall_rules
    # The iterator variable defaults to the block name ("ingress"), or can be customized:
    iterator = rule 
    
    content {
      description = rule.value.description
      from_port   = rule.value.port
      to_port     = rule.value.port
      protocol    = "tcp"
      cidr_blocks = [rule.value.cidr]
    }
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

```text
+---------------------------------------------------------------------------------------------------+
|                        HOW DYNAMIC BLOCKS EXPAND AT COMPILE TIME                                  |
+---------------------------------------------------------------------------------------------------+

 INPUT LIST: [ {port: 80}, {port: 443}, {port: 8443} ]
                        │
                        ▼ Compile Time Expansion by HCL Engine
 RESULTING SYNTHESIZED RESOURCE:
 resource "aws_security_group" "application_firewall" {
   ingress {
     from_port = 80, to_port = 80, protocol = "tcp", cidr_blocks = ["0.0.0.0/0"]
   }
   ingress {
     from_port = 443, to_port = 443, protocol = "tcp", cidr_blocks = ["0.0.0.0/0"]
   }
   ingress {
     from_port = 8443, to_port = 8443, protocol = "tcp", cidr_blocks = ["10.0.0.0/8"]
   }
 }
```

---

## 5. Architectural Best Practices for Iteration

1. **Prefer Standalone Resources Over Nested Dynamic Blocks:**
   Whenever a provider supports standalone resources (e.g. `aws_security_group_rule` instead of nested `ingress {}` blocks inside `aws_security_group`), **always use the standalone resource with `for_each`**. Standalone resources decouple rules from the security group lifecycle, preventing cyclic dependencies and timeouts.
2. **Always Convert Lists with `toset()` for `for_each`:**
   `for_each` only accepts maps or sets. If you have a list of strings (e.g. `var.public_subnet_names`), pass it through `toset(var.public_subnet_names)`.
3. **Never Iterate over Sensitive Maps with `count`:**
   Always use `for_each` when creating IAM users, database accounts, or security-sensitive resources to guarantee auditability and prevent accidental recreation.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Locals Expressions and Built in Functions](./04-Locals-Expressions-and-Built-in-Functions.md) | [Index](../../../README.md) | [06 - Type Constraints Complex Types and any →](./06-Type-Constraints-Complex-Types-and-any.md) |
