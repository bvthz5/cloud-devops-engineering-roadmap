# 03 — Commands, Comments, and Syntax

## 1. Enterprise Script Structure
Standard enterprise scripts start with shebang, strict mode flags (`set -euo pipefail`), global constants, function definitions, and entrypoint `main "$@"`.

## 2. Comments Standard
- `#` for single-line notes.
- `: <<'EOF'` for multi-line block comments.

## 3. Command Chaining
- `;`: Sequential execution regardless of exit code.
- `&&`: Executes next command ONLY if previous succeeded.
- `||`: Executes next command ONLY if previous failed.
- `\`: Line continuation for long commands.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Shebang & Permissions](./02-Shebang-Execution-and-Permissions.md) | [README](./README.md) | [04 - Variables & Quoting](./04-Variables-Environment-and-Quoting.md) |
