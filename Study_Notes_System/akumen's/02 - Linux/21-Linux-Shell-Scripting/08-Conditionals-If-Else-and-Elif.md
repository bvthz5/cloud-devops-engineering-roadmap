# 08 — Conditionals: If, Else, and Elif

## 1. Syntax
```bash
if [[ condition ]]; then
  # commands
elif [[ condition2 ]]; then
  # commands
else
  # commands
fi
```

## 2. Short-Circuit Execution
- `cmd1 && cmd2`: Runs `cmd2` if `cmd1` succeeds.
- `cmd1 || cmd2`: Runs `cmd2` if `cmd1` fails.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - File & String Tests](./07-File-String-and-Numeric-Tests.md) | [README](./README.md) | [09 - Exit Status & Error Handling](./09-Exit-Status-Error-Handling-and-Strict-Mode.md) |
