# Topic 21 — Linux Shell Scripting (Detailed Edition)

## Objective
Master Linux Shell & Bash scripting from foundational concepts to advanced production DevOps automation. This comprehensive 32-file study package follows the standard pedagogical framework: **Concept → Explanation → Syntax → Step-by-Step → Examples → Real-World Use → Troubleshooting → Hands-on Labs → Interview Q&A → MCQs → Quick Revision → Production Checklist**.

## Complete Module Structure (32 Markdown Files)

1. **`01-Shell-and-Bash-Fundamentals.md`**: Concept of CLI shells, kernel-shell interface, taxonomy (`sh`, `bash`, `dash`, `zsh`), login vs non-login shells, interactive vs non-interactive sessions, initialization files (`/etc/profile`, `~/.bashrc`).
2. **`02-Shebang-Execution-and-Permissions.md`**: Shebang mechanics (`#!/usr/bin/env bash`), kernel invocation flow, execution permissions (`chmod +x`), step-by-step execution comparison (`./script.sh` vs `bash` vs `source`).
3. **`03-Commands-Comments-and-Syntax.md`**: Enterprise script structure, commenting standards, command chaining operators (`;`, `&&`, `||`), multi-line continuation (`\`), statement termination rules.
4. **`04-Variables-Environment-and-Quoting.md`**: Local vs global vs exported environment variables, naming rules, quoting mechanics (Single `'...'` vs Double `"..."` vs Unquoted), word splitting, parameter expansion operators.
5. **`05-User-Input-and-Interactive-Scripts.md`**: Capturing input with `read`, prompts (`-p`), secret password prompts (`-s`), timeouts (`-t`), array input (`-a`), raw mode (`-r`), default fallbacks.
6. **`06-Command-Substitution-and-Arithmetic.md`**: Capturing command outputs `$()` vs backticks, integer arithmetic `$(( ... ))`, increment/decrement, floating-point calculations with `bc`.
7. **`07-File-String-and-Numeric-Tests.md`**: Single bracket `[` vs double bracket `[[`, file operators (`-f`, `-d`, `-e`, `-r`, `-w`, `-x`, `-s`), string tests (`-z`, `-n`, `=~`), numeric comparison operators.
8. **`08-Conditionals-If-Else-and-Elif.md`**: Syntax of `if...then...elif...else...fi`, nested logic, logical AND/OR (`&&`, `||`), short-circuit evaluation, system pre-flight checks.
9. **`09-Exit-Status-Error-Handling-and-Strict-Mode.md`**: Exit codes (`$?`), standard error code matrix, explicit `exit N`, strict mode (`set -euo pipefail`), handling expected command failures.
10. **`10-Script-Arguments-and-Getopts.md`**: Positional parameters (`$0`..`$9`, `${10}`), special variables (`$#`, `$*`, `$@`), parameter shifting (`shift`), option parsing using `getopts`.
11. **`11-IO-Redirection-Pipes-and-Process-Substitution.md`**: File descriptors (0: stdin, 1: stdout, 2: stderr), redirection (`>`, `>>`, `2>&1`, `&>`), Heredocs (`<<EOF`), process substitution (`<(cmd)`).
12. **`12-Loops-For-While-Until-and-Case.md`**: `for` loops (lists, ranges, C-style), `while` loops, reading files line-by-line (`while read -r line`), polling, `until` loops, `case...esac` pattern matching.
13. **`13-Functions-Scope-and-Arrays.md`**: Declaring functions, return status vs stdout output, `local` variable scope, indexed arrays, associative dictionaries (`declare -A`).
14. **`14-Text-Processing-Grep-Sed-Awk-in-Scripts.md`**: Integrating `grep` (pattern filter), `sed` (stream editor/substitution), `awk` (column processing), `cut`, `tr`, `sort`, `uniq` into shell pipelines.
15. **`15-Safe-File-Operations-Mktemp-and-Locking.md`**: Preventing symlink vulnerabilities, `mktemp` usage, file locking with `flock`, atomic file updates (`mv`).
16. **`16-Logging-Debugging-and-Traps.md`**: Structured logging functions, ANSI colors, `logger` (syslog/journald), signal traps (`trap`), debug tracing (`set -x`, `PS4`), static analysis with `ShellCheck`.
17. **`17-Cron-Jobs-and-Systemd-Timers-Setup-and-Fixes.md`**: Complete Crontab syntax, environment pitfalls, systemd `.service` and `.timer` setup, step-by-step troubleshooting.
18. **`18-DevOps-Case-Studies-01-to-05.md`**: Real-world scripts 1-5 (`rsync` automated backup, automated server maintenance, disk space monitor & alerter, log cleanup & retention, bulk user creation).
19. **`19-DevOps-Case-Studies-06-to-10.md`**: Real-world scripts 6-10 (system health HTML report, MySQL database backup, website availability monitor, SSL certificate expiry checker, SSH authentication log analyzer).
20. **`20-DevOps-Case-Studies-11-to-15.md`**: Real-world scripts 11-15 (CPU/memory usage monitor, directory archival & compression, bulk file renamer, network latency checker, failed service auto-restarter).
21. **`21-DevOps-Case-Studies-16-to-20.md`**: Real-world scripts 16-20 (Git deployment automation, two-way directory sync, hardware/system info report, process watchdog, Docker container & prune manager).
22. **`22-Hands-On-Labs-01-to-05-Backup-and-Disk.md`**: Step-by-step labs 1-5 (building, executing, and testing backup & storage management scripts).
23. **`23-Hands-On-Labs-06-to-10-Log-and-Security.md`**: Step-by-step labs 6-10 (building, executing, and testing security, log parsing, and network monitors).
24. **`24-Hands-On-Labs-11-to-15-Network-and-DB.md`**: Step-by-step labs 11-15 (building, executing, and testing database backups, uptime checks, and service recovery).
25. **`25-Hands-On-Labs-16-to-20-DevOps-and-Cloud.md`**: Step-by-step labs 16-20 (building, executing, and testing Docker automation, Git pipelines, and cloud bootstrap scripts).
26. **`26-Troubleshooting-Scenarios-and-Common-Pitfalls.md`**: 15 detailed production failure scenarios (CRLF line endings, word splitting, hanging pipes, permission bugs) with root causes and fixes.
27. **`27-Comprehensive-Interview-Questions-and-Answers.md`**: Senior DevOps/SRE scenario and technical interview questions with complete answer rationales.
28. **`28-MCQs-and-Scenario-Quizzes.md`**: Multiple-choice questions with answer keys, code output analysis, and explanations.
29. **`29-Quick-Revision-Notes.md`**: Flashcard-style summary, operator cheat tables, and core rules.
30. **`30-Complete-Command-Cheat-Sheet.md`**: Syntax cheat sheet for Bash builtins, test operators, parameter expansions, and text processing tools.
31. **`31-Production-Scripting-Checklist.md`**: 25-point readiness checklist for script safety, security, portability, and maintainability.
32. **`SOURCE.md`**: Complete preservation of original source notes, real-world case study requirements, and reference URLs.
