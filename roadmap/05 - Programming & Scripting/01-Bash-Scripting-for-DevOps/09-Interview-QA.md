# 09 - Bash Scripting: Interview Questions & Answers

### Q1: What does `set -euo pipefail` do, and why is it essential?
**Answer:**
- `-e`: Exits immediately if any command returns non-zero status.
- `-u`: Exits if an undefined variable is referenced, preventing accidental destructive operations.
- `-o pipefail`: Ensures a pipeline fails if any command in the pipeline fails, rather than only reporting the exit status of the final command.

### Q2: What is the difference between single brackets `[ ... ]` and double brackets `[[ ... ]]`?
**Answer:** Single brackets `[ ... ]` invoke the POSIX standard test command and suffer from word-splitting and pathname expansion issues if variables are unquoted. Double brackets `[[ ... ]]` are a Bash keyword featuring advanced functionality: native regex matching (`=~`), string pattern matching (`==`), logical operators (`&&`, `||`), and safety against unquoted variable splitting.

### Q3: How do you ensure temporary files are deleted even if a script is killed with `Ctrl+C`?
**Answer:** By implementing a cleanup function and registering it with the `trap` built-in targeting `EXIT`, `SIGINT`, and `SIGTERM`:
```bash
trap cleanup EXIT SIGINT SIGTERM
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
