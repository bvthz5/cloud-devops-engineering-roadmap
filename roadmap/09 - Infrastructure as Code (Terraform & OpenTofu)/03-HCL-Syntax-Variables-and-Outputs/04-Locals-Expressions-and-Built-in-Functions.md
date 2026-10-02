# 04 - Locals, Expressions & Built-in Functions

## 1. Local Values

```hcl
locals {
  env_prefix    = "${var.project}-${var.environment}"
  common_tags   = {
    Project     = var.project
    Environment = var.environment
    ManagedBy   = "terraform"
  }
}

resource "aws_instance" "web" {
  tags = merge(local.common_tags, {
    Name = "${local.env_prefix}-web"
  })
}
```

## 2. Essential Built-in Functions

### String Functions
```hcl
upper("hello")                   # "HELLO"
lower("HELLO")                   # "hello"
format("Hello, %s!", "world")    # "Hello, world!"
join("-", ["web", "prod", "01"]) # "web-prod-01"
split(",", "a,b,c")             # ["a", "b", "c"]
replace("hello-world", "-", "_") # "hello_world"
trimspace("  hello  ")          # "hello"
substr("hello", 0, 3)           # "hel"
regex("^([a-z]+)-([0-9]+)$", "web-01")  # ["web", "01"]
```

### Collection Functions
```hcl
length([1, 2, 3])               # 3
contains(["a", "b"], "a")       # true
flatten([[1, 2], [3, 4]])       # [1, 2, 3, 4]
merge({a = 1}, {b = 2})         # {a = 1, b = 2}
keys({a = 1, b = 2})            # ["a", "b"]
values({a = 1, b = 2})          # [1, 2]
lookup({a = 1}, "b", "default") # "default"
zipmap(["a", "b"], [1, 2])      # {a = 1, b = 2}
distinct(["a", "b", "a"])       # ["a", "b"]
sort(["c", "a", "b"])           # ["a", "b", "c"]
```

### Numeric Functions
```hcl
min(1, 2, 3)         # 1
max(1, 2, 3)         # 3
ceil(4.2)            # 5
floor(4.8)           # 4
abs(-10)             # 10
```

### Encoding Functions
```hcl
base64encode("hello")           # "aGVsbG8="
base64decode("aGVsbG8=")        # "hello"
jsonencode({key = "value"})     # '{"key":"value"}'
jsondecode('{"key":"value"}')   # {key = "value"}
yamlencode({key = "value"})     # "key: value\n"
```

### Filesystem Functions
```hcl
file("${path.module}/scripts/init.sh")          # Read file contents
templatefile("${path.module}/tmpl.tftpl", {      # Template rendering
  name = "web"
})
fileexists("${path.module}/optional.txt")         # true/false
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Outputs & Data Sources](./03-Outputs-Data-Sources-and-Cross-Module-References.md) | [README](./README.md) | [05 - Dynamic Blocks & Iteration](./05-Dynamic-Blocks-for_each-count-and-Iteration.md) |
