# CLI and Terminal Basics — Core Concepts

## 1. Terminal vs. Terminal Emulator vs. Shell vs. Command

```text
┌─────────────────────────────────────────────────────────────┐
│ Terminal Emulator (GUI App: Windows Terminal, iTerm2)       │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ Shell (Command Interpreter: Bash, Zsh, PowerShell)     │  │
│  │   ┌─────────────────────────────────────────────────┐ │  │
│  │   │ Executable Command (grep, docker, kubectl, ls)   │ │  │
│  │   └─────────────────────────────────────────────────┘ │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

- **Terminal Emulator:** Graphical desktop window capturing keystrokes and displaying text.
- **Shell:** Command interpreter parsing commands, expanding variables/wildcards, and launching processes.
- **Command:** An instruction, builtin function, or binary executable passed to the shell.

---

## 2. Command Structure Anatomy

```text
command   [options/flags]    [arguments]
   │             │                │
  ls           -lah           /var/log
```

- **Options/Flags:** Modify command behavior (`-l` short format, `--human-readable` long format).
- **Arguments:** Targets operated upon by the command (filenames, directories, URLs, IP addresses).

---

## 3. Path Navigation

- **Absolute Path:** Starts from filesystem root `/`. Always points to the exact same file regardless of current working directory (e.g., `/etc/nginx/nginx.conf`).
- **Relative Path:** Interpreted relative to current working directory (e.g., `./config/app.yaml`).
- **Special Shortcuts:**
  - `.` — Current working directory.
  - `..` — Parent directory.
  - `~` — Current user's home directory (`/home/username` or `/root`).

---

## 4. `PATH` Variable & Exit Status Codes

- **`PATH` Environment Variable:** Colon-separated list of directories searched sequentially when you type a command without an explicit path.
- **Exit Status Code (`$?`):** Integer returned by every command upon completion:
  - `0` $\rightarrow$ **Success** (clean execution).
  - `1 - 255` $\rightarrow$ **Failure / Error** (e.g. `1` general error, `127` command not found, `130` terminated by Ctrl+C).

---

## 5. Streams, Redirection & Pipes

```text
                       ┌─────────────┐
           stdin  ───> │             │ ───> stdout (1)
         (FD 0)        │   Command   │
                       │             │ ───> stderr (2)
                       └─────────────┘
```

### Redirection Operators

- `>` — Overwrites `stdout` to file (`echo "hi" > file.txt`).
- `>>` — Appends `stdout` to file (`echo "hi" >> file.txt`).
- `2>` — Redirects `stderr` to file (`ls /invalid 2> error.log`).
- `2>&1` or `&>` — Merges `stderr` into `stdout`.
- `<` — Feeds file contents into `stdin`.

### Pipeline (`|`)

Connects the `stdout` (FD 1) of the left command to the `stdin` (FD 0) of the right command.

```bash
ps aux | grep nginx | awk '{print $2}'
```

---

## 6. Shell Expansion, Quoting & Chaining

- **Wildcards:** `*` (matches zero or more characters), `?` (matches single character), `[abc]` (matches character set).
- **Single Quotes (`'...'`):** Disables ALL shell expansions (preserves literal string).
- **Double Quotes (`"..."`):** Allows variable expansion (`$VAR`) and command substitution (`$(cmd)`), but preserves spaces.
- **Command Substitution:** `$(command)` executes `command` and embeds its output.
- **Chaining Operators:**
  - `;` — Run next command unconditionally.
  - `&&` — Run next command ONLY if previous returned exit code `0`.
  - `||` — Run next command ONLY if previous returned non-zero exit code.
