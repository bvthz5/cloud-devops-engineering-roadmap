# 04 - Error Handling, Traps, and Signal Management

## 1. Signal Handling with `trap`

Production scripts often create temporary files, mount volumes, or acquire lockfiles. If an engineer hits `Ctrl+C` (`SIGINT`) or Kubernetes sends `SIGTERM`, an un-trapped script leaves orphaned files and stale locks!

```bash
#!/usr/bin/env bash
set -euo pipefail

TMP_DIR=$(mktemp -d /tmp/deploy-XXXXXX)
LOCK_FILE="/var/run/my-script.lock"

cleanup() {
    local EXIT_CODE=$?
    echo "Executing cleanup tasks..."
    rm -rf "${TMP_DIR}"
    rm -f "${LOCK_FILE}"
    echo "Cleanup complete. Exiting with status ${EXIT_CODE}."
    exit "${EXIT_CODE}"
}

# Trap EXIT (always runs on exit), SIGINT (Ctrl+C), and SIGTERM (K8s kill)
trap cleanup EXIT SIGINT SIGTERM

echo "Working with temp directory: ${TMP_DIR}"
# Script logic continues safely...
```

---

## 2. Tracing the Exact Failing Line on Error

```bash
# Print failing line number on unexpected exit
trap 'echo "ERROR: Command failed at line $LINENO with exit code $?" >&2' ERR
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - CLI Argument Parsing with getopts](./03-CLI-Argument-Parsing-with-getopts.md) | [Index](../../../README.md) | [05 - Stream Processing with jq awk and sed →](./05-Stream-Processing-with-jq-awk-and-sed.md) |
