# 02 — Shebang, Execution, and Permissions

## 1. Shebang Mechanics (`#!/usr/bin/env bash`)
The Shebang is the byte sequence `#!` at line 1, column 1 of a script file. When executed, the Linux kernel reads the interpreter path specified after `#!`.

- Portable format: `#!/usr/bin/env bash` (locates `bash` via system `$PATH`).
- Fixed format: `#!/bin/bash` (fails if `bash` is installed in non-standard locations).

## 2. Execution Permission (`chmod +x`)
Linux requires execution permissions (`x`) on a script file for direct kernel invocation.

```bash
# Grant execution permissions
chmod +x script.sh

# Run directly
./script.sh
```

## 3. Execution Methods Comparison
- `./script.sh`: Runs in a child subshell using shebang interpreter.
- `bash script.sh`: Bypasses shebang, runs in child subshell.
- `source script.sh`: Runs inside active parent shell process.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Shell & Bash Fundamentals](./01-Shell-and-Bash-Fundamentals.md) | [README](./README.md) | [03 - Commands & Syntax](./03-Commands-Comments-and-Syntax.md) |
