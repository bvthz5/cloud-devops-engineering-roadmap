# 29 — Quick Revision Notes

## Core Rules & Golden Principles

1. **Shebang Rule**: Start every portable script with `#!/usr/bin/env bash` on Line 1, Byte 0.
2. **Strict Mode Header**: Always include `set -euo pipefail` near top of script.
3. **Double Quote Variables**: Wrap variable expansions in double quotes (`"$VAR"`) to prevent word splitting.
4. **Use Double Brackets**: Prefer `[[ ... ]]` over single brackets `[ ... ]` for tests.
5. **Stderr Redirection**: Send log and error output to stderr (`echo "error" >&2`).
6. **Trap Cleanup**: Always register a signal handler (`trap cleanup EXIT INT TERM`) when creating temporary files.
7. **File Locking**: Use `flock` for scripts executed via Cron to prevent race conditions.
8. **Subshell Avoidance**: Use process substitution `< <(cmd)` instead of `cmd | while read` when modifying outer variables inside loops.
9. **Atomic Overwrites**: Never overwrite active production configs directly; write to a temp file and `mv` atomically.
10. **Lint Before Push**: Run `shellcheck script.sh` and `bash -n script.sh` prior to code review.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [28 - MCQs & Quizzes](./28-MCQs-and-Scenario-Quizzes.md) | [README](./README.md) | [30 - Command Cheat Sheet](./30-Complete-Command-Cheat-Sheet.md) |
