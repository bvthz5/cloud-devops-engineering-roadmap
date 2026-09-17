# 27 — Comprehensive Interview Questions and Answers

## 1. Core Technical & Conceptual Questions

### Q1: What is the difference between login shells and non-login shells?
**Answer**:
- A **login shell** is started when a user authenticates to the system (e.g. SSH session or console login). It executes `/etc/profile`, then the first matching file among `~/.bash_profile`, `~/.bash_login`, or `~/.profile`.
- A **non-login shell** is started after logging in (e.g. opening a GUI terminal window or running `bash`). It executes `~/.bashrc` and `/etc/bash.bashrc`.

---

### Q2: What is the difference between `sh`, `bash`, and `dash`?
**Answer**:
- `sh` (Bourne Shell) is the historical POSIX standard shell interface.
- `bash` (Bourne Again Shell) is an enhanced interactive and scripting shell featuring arrays, process substitution, regex matching, and rich history builtins.
- `dash` (Debian Almquist Shell) is an ultra-fast, lightweight POSIX-compliant shell used in Ubuntu/Debian for system boot scripts (`/bin/sh`).

---

### Q3: How do you make a shell script idempotent?
**Answer**:
An idempotent script produces the same outcome regardless of how many times it is run.
- Use `mkdir -p` instead of `mkdir`.
- Use `rm -f` instead of `rm`.
- Check resource existence (`id -u user &>/dev/null || useradd user`) before creation.
- Check file content with `grep -q` before appending configuration lines.

---

### Q4: Explain parameter expansion `${VAR:-default}` vs `${VAR:=default}` vs `${VAR:?error}`.
**Answer**:
- `${VAR:-default}`: Evaluates to `default` if `VAR` is unset/empty, but leaves `VAR` unchanged.
- `${VAR:=default}`: Evaluates to `default` AND assigns `default` to `VAR` if it was unset/empty.
- `${VAR:?error}`: Prints `error` to stderr and aborts script execution if `VAR` is unset/empty.

---

### Q5: What is the difference between process substitution `<(cmd)` and command substitution `$(cmd)`?
**Answer**:
- Command substitution `$(cmd)` executes `cmd` and replaces the syntax with its **stdout string output**.
- Process substitution `<(cmd)` executes `cmd` and replaces the syntax with a **named pipe / temporary file descriptor path** (`/dev/fd/63`), allowing utilities expecting file paths (like `diff`) to process stream outputs directly.

---

### Q6: How do you capture errors separately from normal stdout in shell scripts?
**Answer**:
Redirect standard output (FD 1) and standard error (FD 2) to separate files or streams:
```bash
/path/to/command > stdout.log 2> stderr.log
```
To print custom error logs to stderr inside script functions:
```bash
echo "Error message" >&2
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [26 - Troubleshooting Scenarios](./26-Troubleshooting-Scenarios-and-Common-Pitfalls.md) | [README](./README.md) | [28 - MCQs & Quizzes](./28-MCQs-and-Scenario-Quizzes.md) |
