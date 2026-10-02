# 15 — Technical Interview Questions & Answers (Junior to Staff SRE)

A comprehensive compilation of technical interview questions testing Command-Line Interface mechanics, Bash internals, stream redirection, defensive scripting, and process execution from Junior DevOps to Senior/Staff SRE levels.

---

### Q1: What is the architectural difference between a Terminal Emulator and a Shell?
**Level:** Junior / Intermediate  
**Answer:**
- **Terminal Emulator (UI layer):** A graphical application (e.g., Windows Terminal, Alacritty, iTerm2) responsible for drawing windows, rendering fonts, handling GPU acceleration, capturing keyboard keystrokes, and translating mouse events. It passes raw byte streams to and from a pseudo-terminal device (`/dev/pts/X`).
- **Shell (Interpretation layer):** A program (e.g., Bash, Zsh, PowerShell) running inside the pseudo-terminal that parses user text input, expands variables and globs, resolves command paths via `$PATH`, and invokes operating system system calls (`fork`, `execve`, `wait4`) to execute binaries.

---

### Q2: Why MUST the `cd` command be implemented as a shell built-in rather than an external binary in `/bin/cd`?
**Level:** Intermediate  
**Answer:**
In Linux, process environments are strictly isolated: **a child process can never alter its parent process's working directory or environment variables**.
If `cd` were an external binary:
1. The shell would `fork()` a child process.
2. The child process would execute `chdir("/var/log")`.
3. The child process would complete its execution and terminate (`exit(0)`).
4. Control would return to the parent shell, which would remain in the exact same original directory.
Therefore, operations that modify the shell's own state (such as `cd`, `export`, `ulimit`, `exit`, and `source`) must execute internally within the shell's own process space as built-in commands.

---

### Q3: What is the exact difference between `"$@"` and `"$*"` in Bash?
**Level:** Intermediate  
**Answer:**
When double-quoted:
- **`"$@"` (At symbol):** Expands to an array of individual words: `"$1" "$2" "$3" ...`. Each positional argument preserves internal spaces intact. This is the **universal best practice** for forwarding script arguments.
- **`"$*"` (Asterisk):** Expands to a single concatenated string containing all arguments separated by the first character of `$IFS` (typically a space): `"$1 $2 $3"`.
- **Example:** If arguments are `"hello world"` and `"test"`:
  - `for x in "$@"` loops **2 times**: `"hello world"` and `"test"`.
  - `for x in "$*"` loops **1 time**: `"hello world test"`.

---

### Q4: Explain the purpose of `set -euo pipefail` in CI/CD automation scripts.
**Level:** Senior  
**Answer:**
By default, Bash scripts continue executing even if commands fail or undefined variables are expanded. `set -euo pipefail` establishes a defensive execution harness:
1. **`-e` (`errexit`):** Halts script execution immediately if any command exits with a non-zero status.
2. **`-u` (`nounset`):** Aborts execution immediately if an uninitialized/undefined variable is expanded (prevents catastrophic bugs like `rm -rf "$UNSET_VAR/*"`).
3. **`-o pipefail`:** By default, a pipeline's exit status is determined solely by the last command. `pipefail` ensures that if *any* command in a pipeline fails (e.g., `cmd1 | cmd2`), the pipeline return code reflects the failure.

---

### Q5: What does the syntax `2>&1` mean, and why does the placement order matter in `command > file.log 2>&1`?
**Level:** Intermediate  
**Answer:**
- **Meaning:** `2>&1` redirects File Descriptor 2 (`stderr`) to whatever destination File Descriptor 1 (`stdout`) is currently pointing to.
- **Why Order Matters:**
  - `command > file.log 2>&1`:
    1. First, FD 1 is redirected to `file.log`.
    2. Next, FD 2 is redirected to the destination of FD 1 (which is now `file.log`). Result: both normal output and error logs write to `file.log`.
  - `command 2>&1 > file.log`:
    1. First, FD 2 is redirected to current FD 1 (the terminal screen).
    2. Next, FD 1 is redirected to `file.log`. Result: errors continue printing to the terminal screen, while standard output goes to the file.

---

### Q6: What is the difference between Single Quotes (`'...'`) and Double Quotes (`"..."`)?
**Level:** Junior / Intermediate  
**Answer:**
- **Single Quotes (`'...'` - Hard Quoting):** Preserves the 100% literal value of every character within the quotes. Variable expansion (`$VAR`), command substitution (`$(...)`), glob wildcards (`*`), and escape sequences (`\n`) are completely disabled.
- **Double Quotes (`"..."` - Soft Quoting):** Suppresses word splitting (spaces do not split into separate arguments) and globbing (`*` remains literal), but **allows** variable expansion (`$VAR`), command substitution (`$(...)`), and arithmetic expansion (`$((...))`).

---

### Q7: What does Exit Code 137 indicate in Kubernetes or Docker?
**Level:** Intermediate  
**Answer:**
- In Linux, exit codes above `128` indicate termination by a signal: `Exit Code = 128 + Signal Number`.
- `137 - 128 = 9` (Signal 9 = **`SIGKILL`**).
- In Kubernetes and Docker, Exit Code 137 almost always indicates that the container was forcibly killed by the **Linux Out-Of-Memory (OOM) Killer** for exceeding its cgroup memory limit (`limits.memory`).

---

### Q8: What is the difference between Command Substitution (`$()`) and Process Substitution (`<()`)?
**Level:** Senior  
**Answer:**
- **Command Substitution (`$(cmd)`):** Executes `cmd`, captures its standard output string, and replaces the expression with that text string on the command line:
  ```bash
  TODAY=$(date +%F)
  ```
- **Process Substitution (`<(cmd)`):** Executes `cmd` asynchronously and connects its output to a special temporary file descriptor exposed in `/dev/fd/X` (a named pipe). This allows commands that expect physical file path arguments to read streaming output without writing intermediate files to disk:
  ```bash
  # Diff output of two API endpoints without saving to disk:
  diff <(curl -s https://api.prod/v1) <(curl -s https://api.stage/v1)
  ```

---

### Q9: Why does a variable modified inside a `cat file | while read line; do ... done` loop lose its value outside the loop?
**Level:** Senior  
**Answer:**
- In POSIX/Bash, every component of a pipeline (`cmd1 | cmd2`) executes inside a separate **Subshell (forked child process)**.
- The `while` loop runs in a child subshell. Variable modifications occur within the child's private memory space.
- When the stream ends, the child subshell terminates, and its memory is discarded. The parent shell never saw the updates.
- **Fix:** Feed the file into the loop using input redirection or process substitution so the loop executes in the main shell context:
  ```bash
  while read -r line; do
      COUNT=$((COUNT + 1))
  done < file.txt
  ```

---

### Q10: How do `ripgrep` (`rg`) and `fd` outperform traditional `grep` and `find` in modern DevOps workflows?
**Level:** Intermediate / Senior  
**Answer:**
1. **Multi-Threaded Parallelism:** `rg` and `fd` utilize Rust's crossbeam work-stealing parallelism to scan directory trees across all CPU cores simultaneously.
2. **Respects VCS Ignore Rules:** Both tools automatically read `.gitignore`, `.ignore`, and hidden file rules by default, skipping massive directories like `node_modules`, `.git`, and build caches that choke classic `grep -r`.
3. **Optimized Regex Engines:** `ripgrep` uses SIMD hardware acceleration (AVX-512 / NEON) to search for substring patterns across memory-mapped files at wire speed.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Troubleshooting](./14-Troubleshooting.md) | [README](./README.md) | [16 - Hands On Practice](./16-Hands-On-Practice.md) |
