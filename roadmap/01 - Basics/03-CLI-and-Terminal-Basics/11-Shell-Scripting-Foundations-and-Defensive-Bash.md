# 11 — Shell Scripting Foundations & Defensive Bash Programming

Writing reliable shell scripts for CI/CD pipelines, container entrypoints, and infrastructure provisioning requires moving beyond simple command sequences into **Defensive Shell Programming**.

---

## 1. The Shebang (`#!`)

The first line of any executable script must be the **Shebang**. It informs the operating system kernel loader which interpreter binary to launch to parse the file:

```bash
# Best Practice (Searches $PATH for bash dynamically):
#!/usr/bin/env bash

# Hardcoded (May fail on Alpine Linux, FreeBSD, or custom paths):
#!/bin/bash
```

### Making the Script Executable
```bash
chmod +x deploy.sh
./deploy.sh
```

---

## 2. The Defensive Bash Prelude: `set -euo pipefail`

By default, Bash is notoriously forgiving: it will happily continue executing a script even if a command fails, or if an undefined variable is referenced. In infrastructure automation, this can lead to disastrous data loss (e.g., `rm -rf "$TARGET_DIR/*"` where `$TARGET_DIR` is empty resolves to `rm -rf /*`).

**Every production shell script must begin with this strict-mode prelude:**

```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'
```

### What Each Flag Does:

| Option | Flag | Behavior / Protection |
| :--- | :--- | :--- |
| **`errexit`** | **`-e`** | **Exit on error:** The script terminates immediately if any command returns a non-zero exit code. |
| **`nounset`** | **`-u`** | **Exit on unset variable:** The script aborts immediately if you attempt to expand an undefined variable. |
| **`pipefail`**| **`-o pipefail`**| **Pipeline failure:** By default, pipeline exit code is determined *only by the last command*. With `pipefail`, if `cmd1` fails in `cmd1 \| cmd2`, the pipeline is treated as failed. |
| **`IFS`** | `IFS=$'\n\t'` | Prevents word splitting on spaces, splitting fields only on newlines and tabs. |

---

## 3. Positional Parameters & Special Variables

When a script or function is executed with arguments (`./deploy.sh staging us-east-1 --dry-run`):

| Variable | Value / Meaning | Example for `./deploy.sh staging us-east-1` |
| :--- | :--- | :--- |
| **`$0`** | The name/path used to invoke the script. | `./deploy.sh` |
| **`$1`, `$2`** | First and second positional arguments. | `$1 = staging`, `$2 = us-east-1` |
| **`${10}`** | Tenth argument (braces required above 9).| `${10}` |
| **`$#`** | **Total count** of arguments passed. | `2` |
| **`$@`** | **Array of all arguments** (preserves quoted spaces when written as `"$@"`). | `("staging", "us-east-1")` |
| **`$*`** | Single concatenated string of all arguments. | `"staging us-east-1"` |
| **`$$`** | Process ID (PID) of the script itself. | `4120` |
| **`$?`** | Exit code of the most recently executed command.| `0` |

> **CRITICAL RULE:** Always forward script arguments to other commands using **`"$@"`** (with double quotes). This preserves arguments containing spaces exactly as intended.

---

## 4. Conditionals: `[[ ... ]]` vs `[ ... ]`

Always use modern Bash double brackets **`[[ ... ]]`** rather than legacy POSIX single brackets `[ ... ]`:

```bash
# String Comparisons
if [[ "$ENV" == "production" ]]; then
    echo "Deploying to Production..."
fi

# Regex Pattern Matching (using =~):
if [[ "$VERSION" =~ ^v[0-9]+\.[0-9]+\.[0-9]+$ ]]; then
    echo "Valid semantic version format!"
fi

# File Checks:
if [[ -f "/etc/config.json" ]]; then
    echo "File exists and is a regular file."
fi

if [[ -d "/var/log/app" ]]; then
    echo "Directory exists."
fi

if [[ -z "$API_KEY" ]]; then
    echo "ERROR: API_KEY is empty or unset!" >&2
    exit 1
fi
```

### Common File Condition Flags
- `-f file`: True if file exists and is a regular file.
- `-d dir`: True if directory exists.
- `-s file`: True if file exists and has size greater than zero.
- `-r file`: True if file is readable.
- `-w file`: True if file is writable.
- `-x file`: True if file is executable.
- `-z str`: True if string length is zero (empty).
- `-n str`: True if string length is non-zero.

---

## 5. Loops and Iteration

```bash
# 1. Standard C-style For Loop
for ((i=1; i<=5; i++)); do
    echo "Attempt $i of 5..."
done

# 2. Iterate over Array / List of Strings
REGIONS=("us-east-1" "eu-west-1" "ap-southeast-1")
for REGION in "${REGIONS[@]}"; do
    echo "Provisioning resources in ${REGION}..."
done

# 3. Read File Line-by-Line Safely (Never use 'for line in $(cat file)')
while IFS= read -r LINE || [[ -n "$LINE" ]]; do
    echo "Processing record: $LINE"
done < data.txt
```

---

## 6. Complete Defensive Script Template

Below is a production-grade Bash script template incorporating logging, argument validation, signal traps, safe temporary directory cleanup, and strict mode:

```bash
#!/usr/bin/env bash
# ==============================================================================
# Script: backup-database.sh
# Description: Dumps and uploads compressed database snapshot to S3.
# ==============================================================================

set -euo pipefail
IFS=$'\n\t'

# Colorized logging functions
log_info()  { printf "\e[34m[INFO]  %s: %s\e[m\n" "$(date +%T)" "$*"; }
log_warn()  { printf "\e[33m[WARN]  %s: %s\e[m\n" "$(date +%T)" "$*"; }
log_error() { printf "\e[31m[ERROR] %s: %s\e[m\n" "$(date +%T)" "$*" >&2; }

# Trap cleanup on script termination (EXIT, SIGINT, SIGTERM)
TMP_DIR=$(mktemp -d -t db_backup_XXXXXX)
cleanup() {
    log_info "Cleaning up temporary scratch directory: ${TMP_DIR}"
    rm -rf "${TMP_DIR}"
}
trap cleanup EXIT

# Validate required positional parameters
if [[ $# -lt 2 ]]; then
    log_error "Usage: $0 <DB_NAME> <S3_BUCKET>"
    exit 1
fi

DB_NAME="$1"
S3_BUCKET="$2"
BACKUP_FILE="${TMP_DIR}/${DB_NAME}-$(date +%Y%m%d%H%M%S).sql.gz"

log_info "Starting database backup for '${DB_NAME}'..."
# Perform dump (simulated)
touch "${BACKUP_FILE}"

log_info "Backup created successfully. Size: $(du -sh "${BACKUP_FILE}" | cut -f1)"
log_info "Uploading to s3://${S3_BUCKET}/ ..."
# aws s3 cp "${BACKUP_FILE}" "s3://${S3_BUCKET}/"

log_info "Operation completed successfully!"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Text Processing Pipelines and Data Wrangling](./10-Text-Processing-Pipelines-and-Data-Wrangling.md) | [Index](../../../README.md) | [12 - Modern CLI Replacements and Productivity Tools →](./12-Modern-CLI-Replacements-and-Productivity-Tools.md) |
