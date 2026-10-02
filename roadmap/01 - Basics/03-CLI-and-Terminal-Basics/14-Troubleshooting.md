# 14 — CLI & Shell Troubleshooting Guide

A systematic troubleshooting manual for diagnosing broken shell scripts, command execution failures, invisible whitespace bugs, and subshell scope leaks.

---

## 1. The Script Debugging Toolkit: `set -x` and `set -v`

When a shell script behaves unexpectedly or produces erroneous results, interactive tracing reveals the exact line-by-line execution flow and expanded variable values.

```bash
# 1. Run script in execution trace mode from CLI
bash -x deploy.sh

# 2. Enable trace mode inside a specific section of a script
set -x  # Start tracing here
cp config.yaml /etc/app/
systemctl restart app
set +x  # Stop tracing here
```

### Enhancing Trace Visibility with `$PS4`
By default, `set -x` prefixes output with `+`. You can configure the `$PS4` prompt variable to display the file name and exact line number for every executed command:
```bash
export PS4='+ ${BASH_SOURCE}:${LINENO}: ${FUNCNAME[0]:+${FUNCNAME[0]}(): }'
```
Output:
```text
+ deploy.sh:14: validate_config(): [[ -f /etc/app/config.yaml ]]
+ deploy.sh:15: validate_config(): echo 'Config validated.'
```

---

## 2. Issue 1: "Command Not Found" (Exit Code 127)

### Symptoms
- Running a command outputs `bash: kubectl: command not found`.
- The command works for one user, but fails when executed via `sudo` or inside a cron job.

### Diagnostic Workflow
```bash
# 1. Check if binary exists on disk
find / -name "kubectl" 2>/dev/null

# 2. Check current PATH
echo $PATH

# 3. Check sudo's secure_path (sudo resets PATH by default!)
sudo -V | grep -i "secure path"

# 4. Check if command is an unexported function or alias
type -a my_command
```

### Remediation
1. If the binary is in `/usr/local/bin` but missing from `$PATH`, add it to `~/.bashrc`:
   ```bash
   export PATH="/usr/local/bin:$PATH"
   ```
2. For cron jobs: Cron runs in a minimal environment (`PATH=/usr/bin:/bin`). Always specify the absolute binary path in crontab:
   ```cron
   0 2 * * * /usr/local/bin/backup-runner --all
   ```

---

## 3. Issue 2: Subshell Variable Loss in Pipelines

### Symptoms
A script attempts to calculate a sum or populate an array inside a `while` loop, but the variable is empty outside the loop!

```bash
# Buggy Code:
COUNT=0
cat server_list.txt | while read -r SERVER; do
    COUNT=$((COUNT + 1))
done
echo "Total Servers: $COUNT"
# Output: Total Servers: 0  <-- Why is COUNT still 0?!
```

### Root Cause
In Bash, each command in a pipeline (`|`) runs inside a **separate Subshell (child process)**. The `while` loop executed in a child subshell; when the loop finished, the subshell terminated and its local `COUNT` variable was destroyed. The parent shell's `COUNT` remained `0`.

### Remediation: Use Process Substitution
Process substitution avoids running the `while` loop in a subshell:
```bash
COUNT=0
while read -r SERVER; do
    COUNT=$((COUNT + 1))
done < <(cat server_list.txt)

echo "Total Servers: $COUNT"
# Output: Total Servers: 42 (Correct!)
```

---

## 4. Issue 3: Detecting Hidden Characters & Windows CRLF

### Symptoms
- Scripts throw cryptic errors like `bash: $'\r': command not found` or `syntax error near unexpected token $'\r'`.
- Filenames appear identical in `ls`, but commands fail with `No such file or directory`.

### Diagnostic Commands
```bash
# 1. View non-printable and carriage return characters (CRLF shows as ^M)
cat -v script.sh

# 2. Check file encoding and line terminators
file script.sh
# If output says: "ASCII text, with CRLF line terminators" -> Problem confirmed!

# 3. View exact hex dump of characters
head -n 5 script.sh | xxd
```

### Remediation
Convert file from DOS/Windows format to Unix format:
```bash
# Using sed:
sed -i 's/\r$//' script.sh

# Or using dos2unix:
sudo apt install -y dos2unix
dos2unix script.sh
```

---

## 5. Issue 4: Hanging Pipelines & Deadlocks

### Symptoms
- A script or pipeline freezes indefinitely without printing output or exiting.
- CPU usage is 0%.

### Root Causes & Diagnostics
1. **Command Waiting for Stdin:** A command in the pipeline (like `grep`, `awk`, or `cat`) was invoked without a filename argument and is waiting for keyboard input:
   ```bash
   # Trace what the hanging process is waiting on:
   sudo strace -p <HANGING_PID>
   # If syscall is: read(0, ...) -> Waiting for stdin!
   ```
2. **Full Pipe Buffer:** The kernel pipe buffer is 64 KB. If the reader process terminates or freezes and stops reading, the writer blocks waiting for buffer space.

---

## Diagnostic Cheat Sheet Matrix

| CLI Issue | Diagnostic Tool | First Command to Run | Root Cause |
| :--- | :--- | :--- | :--- |
| **Script logic failure** | Bash trace | `bash -x script.sh` | Trace variable expansion step-by-step |
| **Command missing** | `type` / `echo $PATH` | `type -a <cmd>` | Missing directory in `$PATH` or alias |
| **Variable lost in loop**| Inspect pipeline | Replace `\| while` with `< <(...)` | Subshell process isolation |
| **`$'\r'` Syntax Error** | `file` / `cat -v` | `cat -v <file>` | Windows CRLF line endings |
| **Hanging script** | `strace` | `sudo strace -p <PID>` | Process blocked on `read(0)` or pipe lock |
| **Unset variable error** | `set -u` | Inspect script line | Referenced variable not defined |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - Real World Scenarios](./13-Real-World-Scenarios.md) | [README](./README.md) | [15 - Interview QA](./15-Interview-QA.md) |
