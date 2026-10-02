# 11 — I/O Redirection, Pipes, and Process Substitution

## 1. Standard Streams
- `0`: stdin, `1`: stdout, `2`: stderr.

## 2. Redirection Operators
- `>` (overwrite stdout), `>>` (append stdout), `2>` (stderr), `&>` (both).

## 3. Process Substitution
```bash
diff -u <(cmd1) <(cmd2)
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Script Arguments & Getopts](./10-Script-Arguments-and-Getopts.md) | [README](./README.md) | [12 - Loops & Case](./12-Loops-For-While-Until-and-Case.md) |
