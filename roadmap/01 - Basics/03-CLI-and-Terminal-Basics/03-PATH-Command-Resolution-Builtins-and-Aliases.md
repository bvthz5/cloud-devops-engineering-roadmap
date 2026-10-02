# 03 — PATH, Command Resolution, Built-ins, and Aliases

---

## 1. How the Shell Finds Commands: The `$PATH` Variable

When you type `kubectl get pods` or `terraform plan`, how does the shell know where that program is located on disk?

The shell does not search the entire hard drive (which would take minutes). Instead, it queries the **`$PATH`** environment variable, which contains a **colon-separated (`:`) list of directories** searched in order from left to right.

```bash
echo $PATH
# Sample Output:
# /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
```

```text
User Types: 'nginx'
      │
      ▼ Searches Directories in $PATH Order:
1. /usr/local/sbin/nginx  ──► Not found
2. /usr/local/bin/nginx   ──► Not found
3. /usr/sbin/nginx        ──► FOUND! Executes /usr/sbin/nginx
(Remaining directories /usr/bin, /bin are ignored)
```

- **Lookup Priority:** If two binaries with the same name exist in `/usr/local/bin` and `/usr/bin`, the one in `/usr/local/bin` executes because it appears earlier in `$PATH`.
- **Command Not Found (Exit Code 127):** If the command is not in any directory listed in `$PATH`, the shell returns `bash: command not found` with exit code `127`.

### Modifying `$PATH` Safely
```bash
# Prepend custom directory to PATH (highest priority)
export PATH="/opt/myapp/bin:$PATH"

# Append custom directory to PATH (lowest priority)
export PATH="$PATH:/opt/myapp/bin"
```

---

## 2. Command Resolution Precedence Hierarchy

When a word is entered at the shell prompt, the shell resolves it according to a strict **6-tier precedence hierarchy**:

```text
                  User Enters Command Word
                             │
                             ▼
              1. Is it an ALIAS? (alias ll='ls -lah')
              ├── Yes ──► Substitute alias text
              └── No
                             │
                             ▼
              2. Is it a RESERVED KEYWORD? (if, for, while, function)
              ├── Yes ──► Execute shell language construct
              └── No
                             │
                             ▼
              3. Is it a SHELL FUNCTION?
              ├── Yes ──► Execute in-memory function body
              └── No
                             │
                             ▼
              4. Is it a BUILT-IN COMMAND? (cd, exit, pwd, type)
              ├── Yes ──► Execute directly inside current shell process
              └── No
                             │
                             ▼
              5. Search Directories in $PATH for EXTERNAL BINARY
              ├── Found ──► fork() + execve(/path/to/binary)
              └── Not Found
                             │
                             ▼
              6. Return "Command not found" (Exit Code 127)
```

---

## 3. Built-in Commands vs External Commands

Understanding the difference between built-in commands and external binaries is essential for performance and scripting:

```text
BUILT-IN COMMAND (e.g., 'cd', 'export')
[ Current Shell Process (PID 1000) ] ── Executes internally with zero fork overhead

EXTERNAL COMMAND (e.g., 'ls', 'python3', 'curl')
[ Current Shell (PID 1000) ] ──(fork)──► [ Child Process (PID 1001) ] ──(execve)──► [ Run Binary ]
```

### Deep Dive: Why MUST `cd` Be a Built-in?
Remember from OS fundamentals: **A child process can never alter its parent process's environment or working directory**.
- If `cd` were an external binary (`/bin/cd`):
  1. The shell would `fork()` a child process.
  2. The child process would execute `chdir("/var/log")`.
  3. The child process would terminate.
  4. The parent shell would remain in the exact same original directory!
- Therefore, `cd`, `export`, `ulimit`, `umask`, and `exit` **must execute inside the shell's own process space** as built-in commands.

---

## 4. Introspection Tools: `type`, `which`, `whereis`, and `command`

### 1. `type` (The Gold Standard for Command Introspection)
Always use `type` instead of `which`. `type` tells you *exactly* what the shell will execute:
```bash
type cd
# Output: cd is a shell builtin

type ls
# Output: ls is aliased to `ls --color=auto'

type for
# Output: for is a shell keyword

type docker
# Output: docker is /usr/bin/docker

# Display all occurrences of a command
type -a echo
# Output:
# echo is a shell builtin
# echo is /bin/echo
```

### 2. `which`
Searches strictly inside `$PATH` for executable files.
- **Gotcha:** `which` does not understand shell aliases, keywords, or functions!

### 3. `whereis`
Locates the binary executable, source code files, and manual pages (`man`) for a command on disk:
```bash
whereis nginx
# Output: nginx: /usr/sbin/nginx /etc/nginx /usr/share/nginx /usr/share/man/man8/nginx.8.gz
```

### 4. `command` & `builtin` (Bypassing Aliases and Functions)
If you created a function named `ls` that wraps the real binary, but you need to run the pure, unmodified external command without the wrapper:
```bash
# Bypass aliases and functions to execute the underlying binary directly
command ls

# Force execution of a shell built-in
builtin echo "Hello"
```

---

## 5. Aliases: Shortcuts and Safeguards

An **Alias** is a custom string shortcut mapped to another command or pipeline.

```bash
# Create useful shortcuts
alias k="kubectl"
alias ll="ls -lah --color=auto"
alias tf="terraform"

# Add safety safeguards against accidental file deletions
alias rm="rm -i"
alias cp="cp -i"
alias mv="mv -i"

# View all active aliases
alias

# Remove an alias
unalias k
```

> **Scripting Gotcha:** By default, **aliases are completely disabled inside non-interactive shell scripts** (`shopt -s expand_aliases` must be enabled manually). In automated scripts, always use **functions** or raw binary paths instead of aliases.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Command Syntax Paths and Directory Navigation](./02-Command-Syntax-Paths-and-Directory-Navigation.md) | [Index](../../../README.md) | [04 - Variables Environment and Shell Expansions →](./04-Variables-Environment-and-Shell-Expansions.md) |
