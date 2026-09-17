# 16 — Logging, Debugging, and Traps

## 1. Logging Functions & Syslog (`logger`)
```bash
log_error() { echo "[ERROR] $*" >&2 ; }
logger -t "my_script" "Event message"
```

## 2. Debugging & Traps
- `set -x`: Print commands before execution.
- `trap cleanup EXIT INT TERM`: Register signal handler.
