# 05 — User Input and Interactive Scripts

## 1. The `read` Builtin Command
The `read` command pauses script execution to capture standard input (`stdin`) from the terminal or user keyboard and stores it into specified variables.

### Basic Syntax
```bash
echo -n "Enter target environment (dev/staging/prod): "
read ENV_NAME
echo "Selected environment: $ENV_NAME"
```

## 2. Useful `read` Flags & Options

| Flag | Description | Code Example |
|---|---|---|
| `-p "prompt"` | Display prompt message inline before reading input. | `read -p "Enter server hostname: " HOSTNAME` |
| `-s` | Silent mode. Disables terminal echo (used for passwords/keys). | `read -sp "Enter DB Password: " DB_PASS` |
| `-t seconds` | Timeout after specified seconds. Returns exit code > 128 if timed out. | `read -t 5 -p "Continue? (y/n): " CHOICE` |
| `-n count` | Reads exactly `count` characters without waiting for Enter key. | `read -n 1 -p "Press any key to continue..."` |
| `-a array` | Reads inputs separated by `$IFS` into an indexed array. | `read -a SERVERS -p "Enter servers: "` |
| `-r` | Raw mode. Prevents backslashes (`\`) from acting as escape characters (RECOMMENDED). | `read -r PATH_INPUT` |

## 3. Practical Interactive Input Examples

### Example A: Password Prompt with Silent Input & Confirmation
```bash
#!/usr/bin/env bash
set -euo pipefail

read -rsp "Enter New Password: " PASS1
echo ""
read -rsp "Confirm New Password: " PASS2
echo ""

if [[ "$PASS1" != "$PASS2" ]]; then
  echo "Error: Passwords do not match!" >&2
  exit 1
fi

echo "Password verified successfully."
```

### Example B: Prompt with Default Fallback Value
```bash
#!/usr/bin/env bash

read -rp "Enter Deployment Port [default: 8080]: " INPUT_PORT
PORT="${INPUT_PORT:-8080}"

echo "Deploying on port $PORT"
```

### Example C: User Confirmation Menu with Timeout
```bash
#!/usr/bin/env bash

if read -rt 10 -p "Perform destructive database migration? (y/N): " CONFIRM; then
  case "$CONFIRM" in
    [yY]|[yY][eE][sS])
      echo "Executing migration..."
      ;;
    *)
      echo "Migration aborted by user."
      ;;
  esac
else
  echo -e "\nTimed out waiting for user response (10s). Aborting."
  exit 1
fi
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Variables & Quoting](./04-Variables-Environment-and-Quoting.md) | [README](./README.md) | [06 - Command Substitution & Arithmetic](./06-Command-Substitution-and-Arithmetic.md) |
