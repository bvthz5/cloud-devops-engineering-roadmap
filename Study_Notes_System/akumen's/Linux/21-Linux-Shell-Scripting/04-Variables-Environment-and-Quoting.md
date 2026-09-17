# 04 — Variables, Environment, and Quoting

## 1. Variable Assignment
- No spaces around `=`: `VAR="value"`.
- Use `${VAR}` syntax when appending literals: `${VAR}_suffix`.

## 2. Scope & Exporting
- `LOCAL_VAR="val"`: Local to script session.
- `export ENV_VAR="val"`: Passed down to child subshell processes.
- `readonly CONST="val"`: Immutable constant.

## 3. Quoting Rules
- Single Quotes `'...'`: Hard quote (preserves literal text).
- Double Quotes `"..."`: Soft quote (allows `$VAR` and `$(cmd)` expansion).
- Unquoted: Subject to word splitting and globbing (AVOID).
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Commands & Syntax](./03-Commands-Comments-and-Syntax.md) | [README](./README.md) | [05 - User Input](./05-User-Input-and-Interactive-Scripts.md) |
