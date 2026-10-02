# 06 - Type Constraints, Complex Types & `any`

## 1. Type Hierarchy

```text
Primitive Types:     string, number, bool
Collection Types:    list(type), set(type), map(type)
Structural Types:    object({...}), tuple([...])
Special:             any, optional()
```

## 2. Optional Attributes (Terraform ≥ 1.3)

```hcl
variable "database" {
  type = object({
    engine  = string
    version = string
    port    = optional(number, 5432)    # Default 5432 if not provided
    ssl     = optional(bool, true)      # Default true if not provided
  })
}

# Usage — port and ssl are optional
module "db" {
  source = "./modules/database"
  database = {
    engine  = "postgres"
    version = "15.4"
    # port defaults to 5432
    # ssl defaults to true
  }
}
```

## 3. The `any` Type

```hcl
variable "metadata" {
  type    = any    # Accepts any type
  default = {}
}
```

**Caution:** `any` disables type checking. Use sparingly for truly generic modules.

## 4. Type Conversion

```hcl
# Automatic conversion
tostring(42)         # "42"
tonumber("42")       # 42
tobool("true")       # true

# Collection conversion
tolist(toset(["b", "a", "c"]))   # ["a", "b", "c"] (sorted, deduplicated)
toset(["a", "b", "a"])            # toset(["a", "b"])
tomap({key = "value"})            # {key = "value"}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Dynamic Blocks for_each count and Iteration](./05-Dynamic-Blocks-for_each-count-and-Iteration.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
