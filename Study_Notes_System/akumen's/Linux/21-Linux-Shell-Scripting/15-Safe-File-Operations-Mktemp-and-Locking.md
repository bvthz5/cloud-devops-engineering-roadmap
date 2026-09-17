# 15 — Safe File Operations, Mktemp, and Locking

## 1. Mktemp
```bash
TMP_FILE="$(mktemp /tmp/file.XXXXXX)"
trap 'rm -f "$TMP_FILE"' EXIT
```

## 2. File Locking (`flock`)
```bash
exec 200>/var/run/my.lock
flock -n 200 || exit 1
```
