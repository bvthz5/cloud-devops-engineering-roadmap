# 05 — Wildcards, Globbing, and Quoting Mechanics

---

## 1. Wildcards and Shell Globbing

**Globbing** is the pattern-matching mechanism the shell uses to generate lists of filenames based on wildcard characters.

```text
User Types: rm *.log
     │
     ▼ Shell scans directory for matches:
     ['app.log', 'error.log', 'access.log']
     │
     ▼ Replaces pattern with matching strings:
Executes: rm app.log error.log access.log
```

### Standard POSIX Wildcards

| Wildcard | Description | Example Pattern | Matches |
| :--- | :--- | :--- | :--- |
| **`*`** | Matches **zero or more** characters. | `*.yaml` | `deploy.yaml`, `service.yaml`, `.yaml` |
| **`?`** | Matches **exactly one** character. | `node-?.js` | `node-1.js`, `node-A.js` (NOT `node-10.js`) |
| **`[abc]`** | Matches **any single character** in the set. | `app-[123].log`| `app-1.log`, `app-2.log` |
| **`[a-z]`** | Matches **any single character in the range**.| `file-[0-9].txt`| `file-0.txt` through `file-9.txt` |
| **`[!abc]`** or **`[^abc]`**| Negation: Matches **any character NOT in set**. | `test-[!0-9].py`| Matches `test-a.py` (ignores `test-1.py`) |

### POSIX Character Classes
To ensure scripts work across international language locales without collation bugs:
- `[[:digit:]]`: Any digit (`0-9`).
- `[[:alpha:]]`: Any letter (`a-z`, `A-Z`).
- `[[:alnum:]]`: Any alphanumeric character (`0-9`, `a-z`, `A-Z`).
- `[[:space:]]`: Whitespace (spaces, tabs, newlines).

---

## 2. Extended Globbing (`extglob`) in Bash

For complex file filtering without invoking `find` or `grep`, enable Bash extended globbing:
```bash
shopt -s extglob
```

| Pattern | Description | Example |
| :--- | :--- | :--- |
| **`?(pattern)`** | Matches zero or one occurrence of patterns. | `file?(1|2).txt` |
| **`*(pattern)`** | Matches zero or more occurrences of patterns. | `data*(.tar\|.gz)` |
| **`+(pattern)`** | Matches one or more occurrences of patterns. | `log-+([0-9]).txt` |
| **`@(pattern)`** | Matches exactly one of the specified patterns. | `@(prod\|stage).env` |
| **`!(pattern)`** | **Matches anything EXCEPT the specified pattern.**| `rm !(*.go)` (Delete all non-Go files!) |

> **DevOps Use Case:** Clean up all test files in a folder while preserving the `.env` and `.git` directories:
> ```bash
> shopt -s extglob
> rm -rf !(.env|.git|main.go)
> ```

---

## 3. Quoting Mechanics: Single Quotes vs Double Quotes

Quoting is one of the most critical topics in shell programming. Incorrect quoting causes word splitting bugs, glob expansion disasters, and security vulnerabilities (shell injections).

```text
Quoting Spectrum:
[ Unquoted ] ──────────────► [ Double Quotes "..." ] ──────────────► [ Single Quotes '...' ]
Complete expansion           Soft Quoting:                            Hard Quoting:
- Variable expansion ($VAR)  - Variable expansion ($VAR) [ALLOWED]   - 100% Literal Text
- Command substitution ($()) - Command substitution ($()) [ALLOWED]   - NO variable expansion
- Globbing (*, ?)            - Arithmetic ($((...)))      [ALLOWED]   - NO command substitution
- Word splitting on spaces   - PRESERVES internal spaces & tabs       - NO escape sequences (\n)
                             - NO globbing (*, ?)                     - Everything is verbatim
```

### Detailed Quoting Comparison Matrix

| Evaluation Rule | Unquoted (`$VAR`) | Double Quotes (`"$VAR"`) | Single Quotes (`'$VAR'`) |
| :--- | :--- | :--- | :--- |
| **Variable Expansion** | YES (`$USER` becomes `ubuntu`) | YES (`$USER` becomes `ubuntu`) | **NO** (Prints literal `$USER`) |
| **Command Substitution** | YES (`$(date)` executes) | YES (`$(date)` executes) | **NO** (Prints literal `$(date)`) |
| **Glob Wildcard Expansion**| YES (`*` expands to files) | **NO** (Prints literal `*`) | **NO** (Prints literal `*`) |
| **Word Splitting (Spaces)**| YES (splits on whitespace) | **NO** (retained as single string) | **NO** (retained as single string) |
| **Backslash Escapes (`\`)**| YES | YES (only for `$`, `` ` ``, `"`, `\`) | **NO** (Prints literal `\`) |

---

## 4. Why You MUST Always Double Quote Variables (`"$VAR"`)

Consider a script that processes files whose names contain spaces:
```bash
FILE_NAME="My Project Document.pdf"

# Dangerous (Unquoted):
rm $FILE_NAME
```
### What Happens Behind the Scenes:
1. The shell expands `$FILE_NAME` into `My Project Document.pdf`.
2. The shell applies **Word Splitting** using `$IFS` (Internal Field Separator, which defaults to space/tab/newline).
3. The shell executes `rm` with **three separate arguments**:
   - `rm "My"`
   - `rm "Project"`
   - `rm "Document.pdf"`
4. `rm` fails to find `"My"` and deletes the wrong files or crashes!

### Correct & Secure:
```bash
# Double quotes preserve whitespace and prevent word splitting:
rm "$FILE_NAME"
```

---

## 5. Escape Characters (`\`)

The backslash **`\`** is the shell escape character. It removes the special meaning of the character immediately following it:

```bash
# 1. Print literal dollar sign without variable expansion
echo "The total price is \$100"
# Output: The total price is $100

# 2. Escape literal spaces in file paths (alternative to quoting)
cat My\ Company\ Report.txt

# 3. Line continuation: Split long commands across multiple lines cleanly
docker run -d \
  --name web-app \
  --restart always \
  -p 80:80 \
  -v /var/log/nginx:/var/log/nginx:ro \
  nginx:alpine
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Variables Environment and Shell Expansions](./04-Variables-Environment-and-Shell-Expansions.md) | [README](./README.md) | [06 - Streams Redirection Pipes and FDs](./06-Streams-Redirection-Pipes-and-FDs.md) |
