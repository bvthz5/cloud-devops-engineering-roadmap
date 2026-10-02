# 02 - Variables: Types, Validation & Precedence

## 1. Variable Declaration

```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t3.micro"
  nullable    = false
  sensitive   = false

  validation {
    condition     = contains(["t3.micro", "t3.small", "t3.medium"], var.instance_type)
    error_message = "Instance type must be t3.micro, t3.small, or t3.medium."
  }
}
```

## 2. Type System

| Type | Example | Description |
|---|---|---|
| `string` | `"hello"` | Text value |
| `number` | `42` | Numeric value |
| `bool` | `true` | Boolean |
| `list(type)` | `["a", "b"]` | Ordered collection |
| `set(type)` | `toset(["a", "b"])` | Unordered unique collection |
| `map(type)` | `{ key = "value" }` | Key-value pairs |
| `object({...})` | `{ name = string, port = number }` | Structured type |
| `tuple([...])` | `[string, number, bool]` | Fixed-length typed sequence |
| `any` | | Accept any type |

## 3. Variable Precedence (Lowest → Highest)

```text
1. Default value in variable block          (lowest priority)
2. Environment variable: TF_VAR_<name>
3. terraform.tfvars file (auto-loaded)
4. *.auto.tfvars files (auto-loaded, alphabetical)
5. -var-file=<file> flag
6. -var='<name>=<value>' flag                (highest priority)
```

## 4. Complex Variable Examples

```hcl
# Object variable
variable "vpc_config" {
  type = object({
    cidr_block           = string
    enable_dns_hostnames = bool
    tags                 = map(string)
  })
  default = {
    cidr_block           = "10.0.0.0/16"
    enable_dns_hostnames = true
    tags                 = { Environment = "dev" }
  }
}

# List of objects
variable "subnets" {
  type = list(object({
    name = string
    cidr = string
    az   = string
  }))
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - HCL Fundamentals](./01-HCL-Language-Fundamentals-Blocks-Arguments-Expressions.md) | [README](./README.md) | [03 - Outputs & Data Sources](./03-Outputs-Data-Sources-and-Cross-Module-References.md) |
