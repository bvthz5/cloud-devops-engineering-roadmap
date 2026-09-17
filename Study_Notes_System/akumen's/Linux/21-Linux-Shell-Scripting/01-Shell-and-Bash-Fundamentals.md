# 01 — Shell and Bash Fundamentals

## 1. What is a Shell?
A **shell** is a command-line interpreter (CLI) program that acts as the primary user-space interface between a user (or automated script) and the Linux kernel. When you type a command into a shell session or run a shell script, the shell parses the input, resolves environment paths, executes system calls (`fork`, `execve`), and redirects input/output streams.

```
+-------------------------------------------------------------+
|                      User / Shell Script                    |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|                      Shell Interpreter                      |
|            (Bash / Dash / Zsh / sh parsing logic)           |
+-------------------------------------------------------------+
                               | System Calls (fork, execve)
                               v
+-------------------------------------------------------------+
|                        Linux Kernel                         |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|                   Hardware (CPU, Disk, NIC)                 |
+-------------------------------------------------------------+
```

## 2. Shell Taxonomy & Comparison

| Shell | Path | Purpose / Characteristics | Default On |
|---|---|---|---|
| **sh** (Bourne Shell) | `/bin/sh` | The original UNIX shell. Lightweight, minimal syntax. Often symlinked to Dash or Bash. | Legacy UNIX |
| **bash** (Bourne Again Shell) | `/bin/bash` | Default Linux user shell. Extended features (arrays, process substitution, regex). | Ubuntu, RHEL, Debian |
| **dash** (Debian Almquist Shell) | `/bin/dash` | Ultra-fast, minimal POSIX-compliant shell. Used for `/bin/sh` boot scripts. | Ubuntu `/bin/sh` |
| **zsh** (Z Shell) | `/bin/zsh` | Interactive-rich shell with auto-completion, themes, plugins. Default on macOS. | macOS, Kali Linux |
| **ksh** (Korn Shell) | `/bin/ksh` | Enterprise UNIX standard shell with advanced programming features. | AIX, Solaris |

## 3. Login vs Non-Login & Interactive vs Non-Interactive Shells

 understanding shell execution contexts is vital for debugging environment variable load failures in CI/CD and Cron.

### Interactive Login Shell
- **Trigger**: Logging in via SSH (`ssh user@server`), console, or `su - user`.
- **Initialization Order**:
  1. `/etc/profile`
  2. `~/.bash_profile` OR `~/.bash_login` OR `~/.profile` (first matching file found)
  3. `~/.bashrc` (explicitly sourced by profile files)

### Interactive Non-Login Shell
- **Trigger**: Opening a new terminal window inside a desktop GUI, or running `bash` inside an existing session.
- **Initialization Order**:
  1. `~/.bashrc`
  2. `/etc/bash.bashrc`

### Non-Interactive Non-Login Shell
- **Trigger**: Executing a script (`./script.sh`), running commands via SSH (`ssh user@server "df -h"`), or Cron execution.
- **Initialization Order**:
  1. Reads `$BASH_ENV` (if set). Does **NOT** source `.bashrc` or `.profile` by default!

```bash
# Check if current session is interactive
if [[ $- == *i* ]]; then
  echo "Interactive Session"
else
  echo "Non-Interactive Session (Script/Cron)"
fi
```

## 4. Viewing Available Shells and Current Shell
```bash
# List all installed shells allowed on the system
cat /etc/shells

# Check current user default login shell from passwd database
getent passwd $USER | cut -d: -f7

# Print currently running shell binary
echo $SHELL

# Print active shell process name
ps -p $$ -o comm=
```
