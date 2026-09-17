# 09 — Exit Status, Error Handling, and Strict Mode

## 1. Exit Status ($?)
- `0`: Success.
- `1-255`: Failure exit code.

## 2. Strict Mode
```bash
set -euo pipefail
```
- `-e`: Exit on error.
- `-u`: Exit on unset variable.
- `-o pipefail`: Return pipeline failure exit code.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Conditionals If/Else](./08-Conditionals-If-Else-and-Elif.md) | [README](./README.md) | [10 - Script Arguments & Getopts](./10-Script-Arguments-and-Getopts.md) |
