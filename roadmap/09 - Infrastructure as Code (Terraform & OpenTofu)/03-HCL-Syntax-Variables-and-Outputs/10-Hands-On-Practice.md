# 10 - Hands-On Practice Labs

## Lab 01: Variable Types and Validation

```hcl
# variables.tf
variable "allowed_regions" {
  type    = list(string)
  default = ["us-east-1", "us-west-2", "eu-west-1"]
}

variable "instance_config" {
  type = object({
    type = string
    size = number
  })
  validation {
    condition     = var.instance_config.size >= 1 && var.instance_config.size <= 100
    error_message = "Size must be between 1 and 100."
  }
}
```

```bash
# Test with terraform console
terraform console
> var.allowed_regions[0]
"us-east-1"
> length(var.allowed_regions)
3
```

---

## Lab 02: Dynamic Block Security Group

Create a security group with dynamic ingress rules sourced from a variable map. Verify rules are generated correctly using `terraform plan`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
