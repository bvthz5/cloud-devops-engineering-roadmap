# 03 - CLI Argument Parsing with getopts

## 1. Why `getopts`?
Hardcoding positional arguments (`$1`, `$2`) makes CLI tools brittle and user-unfriendly. `getopts` is POSIX-compliant and handles short flags cleanly.

---

## 2. Production `getopts` Template

```bash
#!/usr/bin/env bash
set -euo pipefail

usage() {
    echo "Usage: $0 -e <env> -n <namespace> [-v]"
    echo "  -e: Target environment (dev|staging|prod)"
    echo "  -n: Kubernetes namespace"
    echo "  -v: Enable verbose output"
    exit 1
}

VERBOSE=false
ENV=""
NAMESPACE=""

while getopts ":e:n:vh" opt; do
    case "${opt}" in
        e) ENV="${OPTARG}" ;;
        n) NAMESPACE="${OPTARG}" ;;
        v) VERBOSE=true ;;
        h) usage ;;
        \?) echo "Invalid option: -${OPTARG}" >&2; usage ;;
        :)  echo "Option -${OPTARG} requires an argument." >&2; usage ;;
    esac
done
shift $((OPTIND -1))

if [[ -z "${ENV}" || -z "${NAMESPACE}" ]]; then
    echo "ERROR: Missing required arguments." >&2
    usage
fi

echo "Deploying to ENV: ${ENV} in NAMESPACE: ${NAMESPACE} (Verbose: ${VERBOSE})"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Variables & Arrays](./02-Variables-Arrays-and-Parameter-Expansion.md) | [README](./README.md) | [04 - Error Handling & Traps](./04-Error-Handling-Traps-and-Signal-Management.md) |
