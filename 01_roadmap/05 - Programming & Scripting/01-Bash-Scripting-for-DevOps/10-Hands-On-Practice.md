# 10 - Hands-On Practice: Building a Production Backup Script

## Lab Scenario
Construct a hardened, automated directory backup script with strict mode, logging, signal trapping, and argument validation.

---

## Lab Steps

### Step 1: Write Backup Script (`backup.sh`)
```bash
cat << 'EOF' > /tmp/backup.sh
#!/usr/bin/env bash
set -euo pipefail

SRC_DIR=""
DEST_DIR="/tmp/backups"

usage() {
    echo "Usage: $0 -s <source_dir> [-d <dest_dir>]"
    exit 1
}

while getopts ":s:d:h" opt; do
    case "${opt}" in
        s) SRC_DIR="${OPTARG}" ;;
        d) DEST_DIR="${OPTARG}" ;;
        *) usage ;;
    esac
done

[[ -z "${SRC_DIR}" ]] && usage
[[ ! -d "${SRC_DIR}" ]] && { echo "Error: ${SRC_DIR} is not a directory." >&2; exit 2; }

mkdir -p "${DEST_DIR}"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
ARCHIVE_NAME="$(basename "${SRC_DIR}")_${TIMESTAMP}.tar.gz"
TARGET_PATH="${DEST_DIR}/${ARCHIVE_NAME}"

echo "Archiving ${SRC_DIR} to ${TARGET_PATH}..."
tar -czf "${TARGET_PATH}" -C "$(dirname "${SRC_DIR}")" "$(basename "${SRC_DIR}")"

echo "Backup successful! Size: $(du -h "${TARGET_PATH}" | cut -f1)"
EOF
chmod +x /tmp/backup.sh
```

### Step 2: Test Backup Execution
```bash
mkdir -p /tmp/my-data && echo "Important Files" > /tmp/my-data/file.txt
/tmp/backup.sh -s /tmp/my-data
ls -lh /tmp/backups/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
