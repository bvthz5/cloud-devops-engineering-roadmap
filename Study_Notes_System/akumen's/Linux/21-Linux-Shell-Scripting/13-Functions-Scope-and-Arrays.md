# 13 — Functions, Scope, and Arrays

## 1. Functions & Scope
```bash
my_func() {
  local VAR="$1"
  echo "Processing $VAR"
}
```

## 2. Arrays
- Indexed: `ARRAY=("a" "b")`
- Associative: `declare -A MAP ; MAP["key"]="val"`
