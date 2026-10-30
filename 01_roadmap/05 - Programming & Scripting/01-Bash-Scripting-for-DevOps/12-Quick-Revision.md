# 12 - Bash Scripting: Quick Revision Cheat Sheet

## Strict Mode Header
```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'
```

## Parameter Expansion Matrix

| Syntax | Behavior |
|---|---|
| `${VAR:-default}` | Return `default` if unset/empty |
| `${VAR:=default}` | Set and return `default` if unset/empty |
| `${VAR:?error_msg}` | Exit with `error_msg` if unset/empty |
| `${VAR#pattern}` | Strip shortest matching prefix |
| `${VAR##pattern}` | Strip longest matching prefix |
| `${VAR%pattern}` | Strip shortest matching suffix |
| `${VAR%%pattern}` | Strip longest matching suffix |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (02-Python-for-DevOps-and-Automation) →](../02-Python-for-DevOps-and-Automation/01-Python-DevOps-Foundations-and-Environment.md) |
