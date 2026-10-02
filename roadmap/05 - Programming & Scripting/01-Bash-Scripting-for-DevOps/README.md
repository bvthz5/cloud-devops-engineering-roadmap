# Module 01: Bash Scripting for DevOps & Infrastructure Automation

Welcome to **Module 01: Bash Scripting for DevOps**. Bash is the lingua franca of Linux servers, container entrypoints, and CI/CD runners. Writing robust, defensive, and production-ready shell scripts is a non-negotiable skill for every DevOps and Cloud Engineer.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Write bulletproof scripts using **Strict Mode** (`set -euo pipefail`) and understand every flag's safety mechanism.
2. Master positional parameters, robust argument parsing with **`getopts`**, and environment variable expansion.
3. Control script flow with conditionals (`[[ ... ]]` vs `[ ... ]`), loops, and arrays.
4. Implement clean error handling, custom exit codes, and **signal interception using `trap`**.
5. Parse and transform complex data streams using pipes, redirection, **`awk`**, **`sed`**, and **`jq`**.
6. Debug failing shell scripts systematically using `set -x`, `bash -n`, and ShellCheck.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Strict Mode & Script Anatomy](./01-Bash-Strict-Mode-and-Script-Anatomy.md) | `set -euo pipefail`, portable shebangs, script initialization |
| 02 | [Variables, Arrays & Parameters](./02-Variables-Arrays-and-Parameter-Expansion.md) | Default values, string slicing, indexed vs associative arrays, scopes |
| 03 | [CLI Argument Parsing with getopts](./03-CLI-Argument-Parsing-with-getopts.md) | Robust flag handling, options with arguments, help text generation |
| 04 | [Error Handling, Traps & Signals](./04-Error-Handling-Traps-and-Signal-Management.md) | Clean exits, cleanup functions on SIGINT/SIGTERM, custom exit codes |
| 05 | [Stream Processing: jq, awk & sed](./05-Stream-Processing-with-jq-awk-and-sed.md) | JSON querying with jq, column slicing with awk, regex substitution with sed |
| 06 | [Subprocesses, Redirection & Subshells](./06-Subprocesses-Redirection-and-Process-Substitution.md) | File descriptors (0, 1, 2), heredocs, `<(...)` process substitution |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Unquoted variable wiping root directory, silent pipeline failures |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Tracing bugs with `set -x`, syntax linting with ShellCheck, subshell traps |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps Bash interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Building an automated backup script with logging, traps, and JSON alerts |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Syntax cheat sheet, parameter expansion matrix, test operator reference |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Programming Track Index](../README.md) | [README](./README.md) | [01 - Strict Mode](./01-Bash-Strict-Mode-and-Script-Anatomy.md) |
