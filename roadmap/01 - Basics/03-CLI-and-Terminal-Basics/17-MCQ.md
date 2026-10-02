# 17 — Multiple Choice Questions (Self-Assessment)

Test your mastery of Command-Line Interfaces, Bash mechanics, stream redirection, defensive scripting, and process execution. Each question contains four options and an expandable answer with a detailed technical explanation.

---

### Q1: What is the primary functional difference between a Terminal Emulator and a Shell?
- **A)** The shell renders fonts on the screen; the terminal emulator parses commands and runs binaries.
- **B)** The terminal emulator provides the graphical window UI and handles keystrokes/display; the shell interprets commands and executes system calls.
- **C)** The terminal emulator runs in Kernel Space; the shell runs in User Space.
- **D)** There is no difference; they are synonymous terms for the command line.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** A terminal emulator (such as Windows Terminal or Alacritty) is a graphical UI application responsible for window rendering, font display, and capturing keyboard input. The shell (like Bash or Zsh) is the command-line interpreter running inside the terminal that parses syntax, expands variables, and invokes kernel syscalls.
</details>

---

### Q2: In Bash, which startup configuration file is read by an Interactive Login Shell?
- **A)** `~/.bashrc` only
- **B)** `/etc/environment` only
- **C)** `/etc/profile`, followed by `~/.bash_profile` (or `~/.bash_login` or `~/.profile`)
- **D)** `$BASH_ENV` only

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** An interactive login shell (such as logging in over SSH) reads `/etc/profile` for system-wide settings, then searches for the first existing user file among `~/.bash_profile`, `~/.bash_login`, and `~/.profile`. Non-login interactive shells (like opening a new tab) read `~/.bashrc`.
</details>

---

### Q3: What is the order of command resolution precedence in Bash when a command name is typed?
- **A)** External PATH binary → Built-in → Shell function → Alias
- **B)** Built-in → External PATH binary → Alias → Shell function
- **C)** Alias → Shell Keyword → Shell Function → Built-in → External PATH binary
- **D)** Shell function → Built-in → Alias → External PATH binary

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Bash resolves command tokens according to strict precedence: Aliases are checked first, followed by reserved keywords (`if`, `for`), then in-memory shell functions, then shell built-in commands (`cd`, `pwd`), and finally the directories listed in `$PATH` for external executable files.
</details>

---

### Q4: Why must the `cd` command be a shell built-in rather than an external binary in `/bin/cd`?
- **A)** Because external binaries run in user space and lack permission to read filesystems.
- **B)** Because a child process created by the shell to run an external binary cannot alter the working directory of its parent shell process.
- **C)** Because changing directories requires Ring 0 kernel privileges.
- **D)** Because external binaries cannot parse relative paths like `..`.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** When an external binary executes, the shell creates a child process via `fork()`. Any changes the child makes to its working directory (`chdir`) affect only the child process and are discarded upon termination. To alter the current shell's own working directory, `cd` must execute internally within the shell's process.
</details>

---

### Q5: What does the parameter expansion `${IMAGE##*/}` evaluate to if `IMAGE="registry.internal/org/app:v1.2"`?
- **A)** `registry.internal`
- **B)** `app:v1.2`
- **C)** `v1.2`
- **D)** `org/app:v1.2`

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The `##` operator strips the **longest matching prefix** from the start of the string. The pattern `*/` matches everything up to the final slash (`registry.internal/org/`), leaving `app:v1.2`.
</details>

---

### Q6: What is the behavior of Single Quotes (`'...'`) in Bash?
- **A)** They allow variable expansion but disable glob wildcards.
- **B)** They preserve the 100% literal value of every enclosed character, completely disabling variable expansion, command substitution, and escape sequences.
- **C)** They convert text to uppercase characters.
- **D)** They execute enclosed text as a background subshell.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Single quotes (`'...'`) enforce strict hard quoting. Every character enclosed between single quotes is treated literally. `$VAR` prints literal `$VAR`, and `$(date)` will not execute.
</details>

---

### Q7: What does the command `command > file.log 2>&1` accomplish?
- **A)** Redirects standard output to `file.log` and discards all errors.
- **B)** Redirects standard error to standard input.
- **C)** Redirects standard output to `file.log`, and redirects standard error to the same destination as standard output, combining both streams into `file.log`.
- **D)** Runs the command twice: once for output and once for errors.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** `>` redirects standard output (FD 1) to `file.log`. `2>&1` instructs the shell to redirect standard error (FD 2) to whatever destination FD 1 is currently pointing to (`file.log`). Both streams are written to the file.
</details>

---

### Q8: What does Exit Code 137 signify in container environments (Kubernetes/Docker)?
- **A)** Command not found.
- **B)** Script aborted due to syntax error.
- **C)** Process terminated with `SIGKILL` (Signal 9, calculated as 128 + 9), typically triggered by the Linux OOM Killer.
- **D)** Process terminated with `SIGTERM` (Signal 15) for graceful shutdown.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** In Unix, exit codes greater than 128 indicate termination by signal (`128 + N`). Code 137 corresponds to `128 + 9 = SIGKILL`. In Kubernetes and container runtimes, this is the hallmark indicator of an Out-Of-Memory (OOM) termination.
</details>

---

### Q9: In the command pipeline `mvn test | tee test.log`, why might a CI/CD step pass even if `mvn test` fails?
- **A)** Because `mvn test` runs in the foreground and `tee` runs in the background.
- **B)** Because Bash pipelines by default return the exit code of only the final command in the pipeline (`tee`), which succeeded with exit code 0.
- **C)** Because `tee` intercepts and rewrites exit codes.
- **D)** Because pipes automatically suppress non-zero exit codes.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** In default Bash, the exit code of a pipeline is determined exclusively by the rightmost command. Since `tee` succeeded (code 0), the pipeline exited with 0. To fix this, enable `set -o pipefail`, which causes the pipeline to inherit the non-zero status of `mvn test`.
</details>

---

### Q10: What does the `-u` (`nounset`) flag do when used in `set -euo pipefail`?
- **A)** Runs all subsequent commands as an unprivileged user.
- **B)** Unsets all exported environment variables.
- **C)** Causes the script to terminate immediately with an error if an uninitialized or undefined variable is referenced.
- **D)** Suppresses standard output logging.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** `set -u` (`nounset`) treats unset variables as an immediate fatal error during expansion, preventing dangerous edge cases such as `rm -rf "$UNSET_DIR/*"`.
</details>

---

### Q11: How do you resume a suspended terminal job in the background after pressing `Ctrl + Z`?
- **A)** `fg %1`
- **B)** `bg %1`
- **C)** `kill -CONT %1`
- **D)** `disown %1`

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** `Ctrl + Z` sends `SIGSTOP` to suspend the active foreground process. Typing `bg %1` sends `SIGCONT` and resumes the job executing asynchronously in the background. `fg %1` would resume it in the foreground.
</details>

---

### Q12: Why is `ripgrep` (`rg`) significantly faster than standard `grep -r` when searching large code repositories?
- **A)** `ripgrep` bypasses the CPU and executes regex directly on the network interface card.
- **B)** `ripgrep` ignores files larger than 1 MB automatically.
- **C)** `ripgrep` is multi-threaded, uses SIMD hardware acceleration, and automatically respects `.gitignore` rules, skipping `node_modules` and hidden files.
- **D)** `ripgrep` converts text files into binary blobs before searching.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Written in Rust, `ripgrep` utilizes work-stealing multi-threading across all CPU cores, leverages SIMD CPU instructions for pattern scanning, and parses `.gitignore` rules to avoid wasting time scanning massive dependency directories.
</details>
