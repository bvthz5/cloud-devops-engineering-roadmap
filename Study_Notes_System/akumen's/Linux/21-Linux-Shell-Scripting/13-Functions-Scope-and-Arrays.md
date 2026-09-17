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
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Loops & Case](./12-Loops-For-While-Until-and-Case.md) | [README](./README.md) | [14 - Text Processing (Grep/Sed/Awk)](./14-Text-Processing-Grep-Sed-Awk-in-Scripts.md) |
