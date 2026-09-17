# 26 — Troubleshooting Scenarios and Common Pitfalls

## 15 Production Failure Scenarios & Solutions

### Scenario 1: Script Fails with `bad interpreter: No such file or directory`
- **Symptom**: `bash: ./script.sh: /bin/bash^M: bad interpreter: No such file or directory`
- **Root Cause**: Script file contains Windows-style CRLF (`\r\n`) line endings instead of Unix LF (`\n`).
- **Fix**: Run `dos2unix script.sh` or `sed -i 's/\r$//' script.sh`.

---

### Scenario 2: Unquoted Variable Causes `ls: cannot access 'file': No such file`
- **Symptom**: Script fails when processing files with spaces in filenames (`my photo.jpg`).
- **Root Cause**: Unquoted variable `$FILE` undergoes word splitting into two separate arguments.
- **Fix**: Wrap variables in double quotes: `ls "$FILE"`.

---

### Scenario 3: Variable Value Lost Outside `while` Loop
- **Symptom**: `COUNT` variable inside `while` loop increments, but returns `0` after loop finishes.
- **Root Cause**: Piping into a loop (`cat file | while read line; do ... done`) spawns a subshell process. Variable modifications in subshell are lost upon subshell exit.
- **Fix**: Use process substitution: `while read line; do ... done < <(cat file)`.

---

### Scenario 4: Command Fails inside Cron but Succeeds in Terminal
- **Symptom**: Script executed manually by user works, but fails silently when triggered by Cron.
- **Root Cause**: Cron environment has a minimal `$PATH` (`/usr/bin:/bin`) and does not source `.bashrc` or `.profile`.
- **Fix**: Declare explicit `PATH` at the top of the crontab file or script, and use absolute paths for binaries (`/usr/bin/rsync`).

---

### Scenario 5: Pipeline Returns Exit Status 0 Despite Command Failure
- **Symptom**: `grep "missing" file.txt | sort` returns exit status 0 even though `grep` failed.
- **Root Cause**: Default pipeline exit status is the exit status of the *last* command (`sort`).
- **Fix**: Enable `set -o pipefail` at the start of the script.

---

### Scenario 6: `set -u` Crashes Script on Positional Parameters
- **Symptom**: `bash: $1: unbound variable` when user runs script without arguments.
- **Fix**: Use parameter default expansion syntax: `ARG1="${1:-default_value}"`.

---

### Scenario 7: Concurrent Executions Corrupt Log File
- **Symptom**: Overlapping cron runs write corrupted interleaved logs.
- **Fix**: Use `flock` to enforce single instance execution: `exec 200>/tmp/lock; flock -n 200 || exit 1`.

---

### Scenario 8: `mktemp` Fails with `Invalid template`
- **Symptom**: `mktemp: invalid template 'tmp.txt'`
- **Fix**: Template string must end with at least 3 consecutive `X` characters: `mktemp /tmp/file.XXXXXX`.

---

### Scenario 9: Floating Point Arithmetic Causes Syntax Error in `[[ ... ]]`
- **Symptom**: `bash: [[: 1.5: syntax error: invalid arithmetic operator`
- **Fix**: Bash integers only. Use `bc` for decimal comparisons: `echo "$LOAD > 1.0" | bc`.

---

### Scenario 10: Infinite Loop in `while` Loop Reading File
- **Symptom**: `while read line` loops infinitely over the last line.
- **Fix**: Ensure loop terminates properly on EOF: `while IFS= read -r line || [[ -n "$line" ]]; do`.

---

### Scenario 11: `sudo` Prompt Fails inside Automated Pipeline
- **Symptom**: `sudo: a terminal is required to read the password`
- **Fix**: Configure passwordless sudo in `/etc/sudoers` for specific command or execute script as root user.

---

### Scenario 12: `sed` Replacement Fails with Path Slash Characters
- **Symptom**: `sed: expression #1, char 9: unknown option to 's'` when replacing path string `/var/log`.
- **Fix**: Use alternative delimiter in `sed`: `sed 's|/old/path|/new/path|g'`.

---

### Scenario 13: Signal `SIGTERM` Ignored in Docker Container
- **Symptom**: Container takes 10 seconds to stop (`docker stop` timeout).
- **Fix**: End entrypoint script with `exec "$@"` to pass PID 1 to application process.

---

### Scenario 14: Subshell Modifies Array, Parent Array Remains Unchanged
- **Symptom**: `ARRAY+=("item")` inside `cmd | while read` loop does not update parent array.
- **Fix**: Avoid piping into loops when building arrays. Use process substitution.

---

### Scenario 15: Single Bracket `[` Fails with `too many arguments`
- **Symptom**: `[ $STR == "hello" ]` fails if `$STR` is empty or contains spaces.
- **Fix**: Replace `[` with double bracket `[[ $STR == "hello" ]]`.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [25 - Hands-On Labs 16-20](./25-Hands-On-Labs-16-to-20-DevOps-and-Cloud.md) | [README](./README.md) | [27 - Comprehensive Interview Q&A](./27-Comprehensive-Interview-Questions-and-Answers.md) |
