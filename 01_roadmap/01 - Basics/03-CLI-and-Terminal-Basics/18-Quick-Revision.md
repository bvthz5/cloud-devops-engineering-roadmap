# 18 — Quick Revision & Reference Cheat Sheet

A condensed, high-yield reference guide summarizing Command-Line Interface mechanics, Bash parameter expansions, stream redirection, and defensive scripting rules.

---

## 1. Streams & Redirection Reference

```text
[stdin:  FD 0] ──► Keyboard / Input Pipe
[stdout: FD 1] ──► Terminal Display Screen
[stderr: FD 2] ──► Terminal Display Screen (Unbuffered)
```

| Syntax | Action / Stream Target | Example |
| :--- | :--- | :--- |
| **`> file`** | Overwrites `file` with standard output (FD 1). | `echo "token" > secret.txt` |
| **`>> file`** | Appends standard output (FD 1) to `file`. | `echo "log entry" >> app.log` |
| **`2> file`** | Redirects standard error (FD 2) only to `file`. | `ls /root 2> errors.log` |
| **`2>&1`** | Merges stream 2 into stream 1. | `deploy.sh > out.log 2>&1` |
| **`&> file`** | Shorthand for redirecting both stdout and stderr. | `npm build &> build.log` |
| **`> /dev/null 2>&1`**| Discards all output completely (silent mode). | `cron_job.sh > /dev/null 2>&1` |
| **`< file`** | Feeds `file` contents into standard input (FD 0). | `mysql db < dump.sql` |
| **`cmd1 \| cmd2`** | Connects `stdout` of cmd1 to `stdin` of cmd2. | `ps aux \| grep nginx` |
| **`tee -a file`** | Splits output: prints to screen AND appends to file. | `echo "data" \| tee -a log.txt` |

---

## 2. Parameter Expansion Cheat Sheet

| Syntax | Action | Example (`FILE="deploy.tar.gz"`) | Output |
| :--- | :--- | :--- | :--- |
| **`${VAR:-def}`** | Use default if empty/unset | `${PORT:-8080}` | `8080` (if empty) |
| **`${VAR:=def}`** | Assign default if empty/unset | `${TIMEOUT:=30}` | Sets & returns `30` |
| **`${VAR:?err}`** | Abort with error message if unset | `${DB_USER:?Username required}` | Halts script |
| **`${#VAR}`** | String length in characters | `${#FILE}` | `13` |
| **`${VAR:pos:len}`**| Substring slice | `${FILE:0:6}` | `deploy` |
| **`${VAR#pat}`** | Trim shortest prefix match | `${FILE#*.}` | `tar.gz` |
| **`${VAR##pat}`**| Trim longest prefix match | `${FILE##*.}` | `gz` |
| **`${VAR%pat}`** | Trim shortest suffix match | `${FILE%.*}` | `deploy.tar` |
| **`${VAR%%pat}`**| Trim longest suffix match | `${FILE%%.*}` | `deploy` |
| **`${VAR//find/rep}`**| Global find and replace | `${FILE//./-}` | `deploy-tar-gz` |

---

## 3. Quoting Rules Summary

| Construct | Unquoted | Double Quotes (`"..."`) | Single Quotes (`'...'`) |
| :--- | :--- | :--- | :--- |
| **`$VAR` (Variables)** | Expands & splits | Expands (preserves spaces) | **Literal text** |
| **`$(cmd)` (Subshell)**| Executes & splits | Executes (preserves spaces)| **Literal text** |
| **`*` (Wildcards)** | Expands to files | **Literal asterisk** | **Literal asterisk** |
| **Spaces in values** | Splits into arguments| Preserved as 1 argument | Preserved as 1 argument |

---

## 4. Standard Exit Codes

| Exit Code | Meaning / Cause |
| :--- | :--- |
| **`0`** | Success / OK |
| **`1`** | General error |
| **`2`** | Shell syntax error or misused built-in |
| **`126`** | Permission denied (file not executable: `chmod +x` needed) |
| **`127`** | Command not found (missing binary or invalid `$PATH`) |
| **`130`** | Terminated by `SIGINT` (`Ctrl+C`, 128 + 2) |
| **`137`** | Terminated by `SIGKILL` (128 + 9) — **Out-Of-Memory (OOM)!** |
| **`139`** | Terminated by `SIGSEGV` (128 + 11) — Segmentation fault |
| **`143`** | Terminated by `SIGTERM` (128 + 15) — Graceful eviction |

---

## 5. History Expansion Shortcuts

- `!!`: Repeat entire last command (`sudo !!`).
- `!$`: Last argument of previous command (`mkdir /path/dir` ──► `cd !$`).
- `!^`: First argument of previous command.
- `!*`: All arguments of previous command.
- `^old^new^`: Quick search-and-replace typo in previous command.
- `Ctrl + R`: Interactive backward incremental history search.

---

## 6. Defensive Bash Script Header

```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'
```
- `-e`: Exit immediately on command failure.
- `-u`: Exit immediately if an unset variable is referenced.
- `-o pipefail`: Return pipeline exit status based on first failed command.

---

## 7. Modern CLI Tools Quick Reference

| Modern Tool | Replaces | Key DevOps Command |
| :--- | :--- | :--- |
| **`ripgrep` (`rg`)** | `grep -r` | `rg -i "api_key" -t yaml` |
| **`fd`** | `find` | `fd -e conf` |
| **`bat`** | `cat` | `bat --style=plain config.yaml` |
| **`eza`** | `ls` | `eza -la --git --tree --level=2` |
| **`fzf`** | `Ctrl+R` | `vim $(fzf)` / `kill -9 $(ps -ef \| fzf \| awk '{print $2}')` |
| **`jq`** | `awk` (JSON) | `kubectl get pods -o json \| jq -r '.items[].metadata.name'` |
| **`yq`** | Python (YAML)| `yq -i '.spec.replicas = 3' deployment.yaml` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 17 - MCQ](./17-MCQ.md) | [Index](../../../README.md) | [Next Module (04-Data-Formats-YAML-JSON-XML-TOML) →](../04-Data-Formats-YAML-JSON-XML-TOML/01-Structured-Data-Serialization-and-Representations.md) |
