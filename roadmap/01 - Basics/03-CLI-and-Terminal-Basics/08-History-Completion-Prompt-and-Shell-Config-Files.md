# 08 — History, Tab Completion, Prompts, and Shell Startup Files

---

## 1. Command History Mastery

The shell records previously executed commands in an in-memory buffer, persisting them to a disk file (`~/.bash_history` for Bash, `~/.zsh_history` for Zsh) upon clean shell exit.

```bash
# Display the last 20 executed commands
history 20

# Search history interactively in real-time
# Press: Ctrl + R, then type search term (e.g., 'docker run')
```

### Essential History Expansion Tricks (Save Hours Daily)

| Shortcut | Expansion / Action | Practical Example |
| :--- | :--- | :--- |
| **`!!`** | **Repeats the entire previous command.** | Ran `apt update` (got permission denied) ──► Run: `sudo !!` |
| **`!$`** | **The last argument of the previous command.**| Ran `mkdir -p /var/log/myapp` ──► Run: `cd !$` |
| **`!^`** | **The first argument of the previous command.**| Ran `cp config.yaml backup.yaml` ──► Run: `vim !^` (edits config.yaml) |
| **`!*`** | **All arguments of the previous command.** | Ran `ls a.txt b.txt c.txt` ──► Run: `rm !*` |
| **`!n`** | **Executes command line number `n` from history.**| `!1042` |
| **`!string`** | **Executes most recent command starting with string.**| `!git` (runs latest git command) |
| **`^old^new^`**| **Quick substitution:** Replaces `old` with `new` in previous command. | Ran `gerp error log.txt` ──► Run: `^gerp^grep^` |

### Tuning Bash History for DevOps (Prevent Truncation)
Add to `~/.bashrc`:
```bash
# Don't store duplicate lines or lines starting with a space
export HISTCONTROL=ignoreboth:erasedups

# Store 100,000 commands in memory and on disk
export HISTSIZE=100000
export HISTFILESIZE=100000

# Append timestamps to history (Format: YYYY-MM-DD HH:MM:SS)
export HISTTIMEFORMAT="%F %T "

# Append history immediately rather than waiting for shell exit
shopt -s histappend
PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"
```

---

## 2. Tab Completion & Programmable Autocompletion

Tab completion speeds up CLI navigation and eliminates typo-induced errors:
- Pressing `Tab` once auto-completes unique commands, filenames, and paths.
- Pressing `Tab` twice lists all matching candidate possibilities.

### Enabling Cloud Tool Autocompletions
Modern DevOps CLIs support rich programmable completion:
```bash
# Kubernetes kubectl autocompletion
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc

# Docker completion
sudo apt install -y bash-completion

# Helm completion
source <(helm completion bash)
```

---

## 3. Shell Prompts & The `$PS1` Variable

The **`$PS1`** (Prompt String 1) environment variable defines the format, colors, and contextual telemetry displayed at your shell prompt.

### Common PS1 Escape Sequences:
- `\u`: Current username.
- `\h`: Hostname up to the first dot.
- `\w`: Current working directory (`~` for home).
- `\W`: Basename of current working directory.
- `\t`: Current time (24-hour `HH:MM:SS`).
- `\$`: Displays `#` if root, `$` for regular users.

### Example Custom Colored Prompt:
```bash
export PS1="\[\e[32m\]\u@\h\[\e[m\]:\[\e[34m\]\w\[\e[m\]\$ "
```

> **Modern Alternative:** Modern SREs widely use **Starship** (`https://starship.rs`), a blazing-fast, cross-shell prompt written in Rust that displays Git branches, Kubernetes contexts, AWS profiles, and Node/Python versions automatically.

---

## 4. Shell Startup Configuration Files Explained

When a shell launches, it reads configuration files in a strict order depending on whether it is a **Login** or **Non-Login** shell:

```text
LOGIN SHELL (SSH, Virtual Console, or 'su -')
1. /etc/profile              (System-wide settings)
2. ~/.bash_profile           (User-specific login settings)
     └─► Typically sources: ~/.bashrc
3. ~/.profile                (Fallback if ~/.bash_profile does not exist)
4. ~/.bash_logout            (Executed on logout/exit)

NON-LOGIN INTERACTIVE SHELL (New terminal tab, Desktop terminal)
1. /etc/bash.bashrc          (System-wide interactive settings)
2. ~/.bashrc                 (User-specific interactive settings: aliases, functions)

ZSH SHELL STARTUP ORDER
1. /etc/zshenv    ──► ~/.zshenv
2. /etc/zprofile  ──► ~/.zprofile (Login only)
3. /etc/zshrc     ──► ~/.zshrc    (Interactive)
4. /etc/zlogin    ──► ~/.zlogin   (Login only)
```

### Standard Pattern: Linking `.bash_profile` to `.bashrc`
To guarantee your aliases and functions load whether you log in via SSH or a GUI terminal, `~/.bash_profile` should always source `~/.bashrc`:

```bash
# Inside ~/.bash_profile:
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Command Chaining Exit Codes and Job Control](./07-Command-Chaining-Exit-Codes-and-Job-Control.md) | [README](./README.md) | [09 - Help Systems and Introspection Tools](./09-Help-Systems-and-Introspection-Tools.md) |
