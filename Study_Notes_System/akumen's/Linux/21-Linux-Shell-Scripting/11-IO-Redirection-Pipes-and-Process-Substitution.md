# 11 — I/O Redirection, Pipes, and Process Substitution

## 1. Standard Streams
- `0`: stdin, `1`: stdout, `2`: stderr.

## 2. Redirection Operators
- `>` (overwrite stdout), `>>` (append stdout), `2>` (stderr), `&>` (both).

## 3. Process Substitution
```bash
diff -u <(cmd1) <(cmd2)
```
