# 08 - Bash Scripting: Troubleshooting Guide

## 1. Systematically Debugging Scripts

```bash
# 1. Syntax check without executing code
bash -n deploy.sh

# 2. Print every command before executing with expanded arguments:
bash -x deploy.sh

# 3. Enable execution trace selectively inside a specific code block:
set -x
critical_database_operation
set +x
```

---

## 2. Static Analysis with ShellCheck

Always lint scripts before committing:
```bash
shellcheck deploy.sh
```
ShellCheck catches unquoted variables, subshell variable leaks, and deprecated test brackets automatically.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
