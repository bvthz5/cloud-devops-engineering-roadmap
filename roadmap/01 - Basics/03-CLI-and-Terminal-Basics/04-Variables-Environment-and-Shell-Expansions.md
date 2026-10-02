# 04 — Variables, Environment, and Shell Expansions

---

## 1. Shell Variables vs Environment Variables

In Linux, variables store text strings and integers. However, there is a fundamental architectural boundary between **Shell Variables** and **Environment Variables**:

```text
CURRENT SHELL PROCESS (PID 1200)
┌────────────────────────────────────────────────────────┐
│ Shell Variables (Local):                               │
│   APP_PORT=8080   (Visible ONLY to current shell)      │
│                                                        │
│ Environment Variables (Exported):                      │
│   export DB_HOST="db.prod.internal"                    │
└──────────────────────────┬─────────────────────────────┘
                           │
             fork() + execve() spawns Child Process
                           │
                           ▼
CHILD PROCESS (PID 1201: Python Script)
┌────────────────────────────────────────────────────────┐
│ Inherited Environment:                                 │
│   DB_HOST="db.prod.internal"  ──► [ VISIBLE ]          │
│                                                        │
│ Local Shell Variables:                                 │
│   APP_PORT                    ──► [ NOT VISIBLE / NULL]│
└────────────────────────────────────────────────────────┘
```

### Demonstrating Variable Scope
```bash
# 1. Define a local shell variable
DATABASE_NAME="production_db"

# Try reading it from a child subshell or python:
python3 -c 'import os; print(os.getenv("DATABASE_NAME"))'
# Output: None (Not visible!)

# 2. Export the variable to the environment
export DATABASE_NAME="production_db"

# Now test again in child process:
python3 -c 'import os; print(os.getenv("DATABASE_NAME"))'
# Output: production_db (Successfully inherited!)
```

### Inspecting Environment Variables
```bash
# Print all exported environment variables
printenv | head -n 15

# Query a single environment variable
printenv HOME

# Run a command with temporary environment variable (without altering current shell)
PORT=9090 node server.js
```

---

## 2. Output Formatting: `echo` vs `printf`

### `echo`
Prints arguments separated by spaces, followed by a newline:
```bash
echo "Hello World"
echo -n "No trailing newline"
echo -e "Line 1\nLine 2\tTabbed" # -e enables backslash escapes
```
> **Portability Warning:** The behavior of `echo -e` and `echo -n` differs between Bash, Zsh, Dash, and macOS BSD `echo`. For robust production scripts, **always use `printf`**.

### `printf` (POSIX Standard Format Specifier)
Behaves like C's `printf`, guaranteeing identical output across all operating systems:
```bash
# Syntax: printf "format_string" [arguments...]
printf "User: %-10s | Role: %-8s | UID: %05d\n" "alice" "admin" 42
# Output: User: alice      | Role: admin    | UID: 00042
```

---

## 3. Command Substitution: `$()` vs Backticks

**Command Substitution** allows the standard output (`stdout`) of a command to replace the command string itself.

```text
CURRENT_DATE=$(date +%Y-%m-%d)
   │
   ▼
1. Shell executes 'date +%Y-%m-%d' in a subshell
2. Strips trailing newline
3. Assigns result string to $CURRENT_DATE
```

### Why `$()` Completely Replaces Legacy Backticks (`` `...` ``)
1. **Nesting Capability:** `$()` can be nested indefinitely without complex backslash escaping:
   ```bash
   # Clean and readable nesting with $():
   TAR_NAME="backup-$(basename $(pwd))-$(date +%s).tar.gz"
   
   # Painful, brittle nesting with backticks:
   TAR_NAME="backup-\`basename \`pwd\`\`-\`date +%s\`.tar.gz" # Prone to syntax errors
   ```
2. **Visual Clarity:** Backticks are easily confused with single quotes (`'`).

---

## 4. Parameter Expansion: The Swiss Army Knife of Bash

Parameter expansion (`${...}`) allows powerful string manipulation, defaults, slicing, and pattern replacement directly inside the shell without launching external processes like `sed`, `awk`, or `cut`.

### Master Parameter Expansion Cheat Sheet

| Syntax | Action / Meaning | Example (`URL="https://api.github.com/v1"`) | Result |
| :--- | :--- | :--- | :--- |
| **`${VAR}`** | Basic variable expansion | `echo "${URL}"` | `https://api.github.com/v1` |
| **`${VAR:-default}`**| **Use Default:** Returns default if VAR is unset or empty | `echo "${ENV:-staging}"` | `staging` (if `$ENV` empty) |
| **`${VAR:=default}`**| **Assign Default:** Sets VAR to default if unset or empty | `echo "${TIMEOUT:=30}"` | Sets and prints `30` |
| **`${VAR:?error}`**  | **Mandatory Check:** Halts script with error message if unset | `echo "${DB_PASS:?Password required!}"`| Crashes if empty |
| **`${VAR:+alternate}`**| **Use Alternate:** Returns value if VAR IS set, else null | `echo "${DEBUG:+"Verbose mode active"}"`| Prints message if debug set |
| **`${#VAR}`** | **String Length:** Returns character count of value | `echo "${#URL}"` | `26` |
| **`${VAR:offset:len}`**| **Substring Slicing:** Extracts characters from offset | `echo "${URL:8:14}"` | `api.github.com` |
| **`${VAR#pattern}`** | **Trim Shortest Prefix:** Strips shortest match from start | `echo "${URL#*/}"` | `/api.github.com/v1` |
| **`${VAR##pattern}`**| **Trim Longest Prefix:** Strips longest match from start | `echo "${URL##*/}"` | `v1` |
| **`${VAR%pattern}`** | **Trim Shortest Suffix:** Strips shortest match from end | `echo "${URL%/*}"` | `https://api.github.com` |
| **`${VAR%%pattern}`**| **Trim Longest Suffix:** Strips longest match from end | `FILE="archive.tar.gz"`; `echo "${FILE%%.*}"` | `archive` |
| **`${VAR/find/replace}`**| **Single Replace:** Replaces first occurrence | `echo "${URL/api/gateway}"` | `https://gateway.github.com/v1` |
| **`${VAR//find/replace}`**| **Global Replace:** Replaces all occurrences | `PATH="/a/b/c"`; `echo "${PATH//\//-}"` | `-a-b-c` |

---

## 5. Tilde Expansion & Filename Expansion

### Tilde Expansion (`~`)
- `~`: Expands to current user's home directory (`/home/ubuntu`).
- `~root`: Expands to root's home directory (`/root`).
- `~+`: Expands to current working directory (equivalent to `$PWD`).
- `~-`: Expands to previous working directory (equivalent to `$OLDPWD`).

### Filename Expansion (Globbing)
The shell scans each command token for wildcard characters (`*`, `?`, `[...]`) and replaces that token with an alphabetically sorted list of matching filenames from the filesystem:
```bash
# Expands to all files ending in .log inside /var/log
ls -l /var/log/*.log
```
> **Order of Operations:** The shell performs Filename Expansion **BEFORE** the command is executed. The command itself (`ls`) never sees the literal asterisk `*`—it receives the already expanded list of file strings!
