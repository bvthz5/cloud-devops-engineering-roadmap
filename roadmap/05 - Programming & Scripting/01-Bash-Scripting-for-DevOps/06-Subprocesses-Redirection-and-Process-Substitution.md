# 06 - Subprocesses, Redirection, and Process Substitution

## 1. Standard Streams & File Descriptors

| Descriptor | Name | Target | Redirection Syntax |
|---|---|---|---|
| `0` | `stdin` | Keyboard / Input Pipe | `< file` |
| `1` | `stdout` | Terminal Output Screen | `> file` or `>> file` (append) |
| `2` | `stderr` | Terminal Error Screen | `2> error.log` |

```bash
# Redirect both stdout and stderr to a single logfile:
./backup.sh > backup.log 2>&1
# Or modern Bash syntax:
./backup.sh &> backup.log

# Discard all output silently:
./noisy_command.sh &> /dev/null
```

---

## 2. Process Substitution: `<(...)`

Avoid creating temporary files on disk when comparing outputs of two commands:
```bash
# Compare pods running in two different namespaces without writing to disk:
diff <(kubectl get pods -n dev -o jsonpath='{.items[*].metadata.name}' | tr ' ' '\n' | sort) \
     <(kubectl get pods -n prod -o jsonpath='{.items[*].metadata.name}' | tr ' ' '\n' | sort)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Stream Processing with jq awk and sed](./05-Stream-Processing-with-jq-awk-and-sed.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
