# 12 - Quick-Revision & Enterprise Cheat Sheet

## HCL Quick Reference

| Element | Syntax |
|---|---|
| String interpolation | `"${var.name}-${var.env}"` |
| Conditional | `var.env == "prod" ? "large" : "small"` |
| for expression (list) | `[for s in var.list : upper(s)]` |
| for expression (map) | `{for k, v in var.map : k => upper(v)}` |
| Splat | `aws_instance.web[*].id` |

## Variable Types

| Type | Example |
|---|---|
| `string` | `"hello"` |
| `number` | `42` |
| `bool` | `true` |
| `list(string)` | `["a", "b"]` |
| `map(string)` | `{key = "val"}` |
| `object({name=string})` | `{name = "web"}` |

## Top 10 Functions

| Function | Purpose |
|---|---|
| `merge()` | Combine maps |
| `join()` | Join list into string |
| `split()` | Split string into list |
| `lookup()` | Map lookup with default |
| `format()` | Printf-style formatting |
| `flatten()` | Flatten nested lists |
| `contains()` | Check if list contains value |
| `length()` | Count elements |
| `file()` | Read file contents |
| `templatefile()` | Render template |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 04 - State Management](../04-Terraform-State-Management-and-Remote-Backend/README.md) |
