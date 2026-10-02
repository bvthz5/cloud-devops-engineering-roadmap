# 10 - Troubleshooting Cross-Platform Integration Issues

When developers write code or scripts on Windows and deploy them to Linux servers, subtle cross-platform incompatibilities can cause deployment failures.

---

## 🚨 1. Issue: `bash: ./script.sh: /bin/bash^M: bad interpreter`

### Root Cause:
Windows editors use **CRLF** line endings (`\r\n`), whereas Linux shell interpreters require **LF** line endings (`\n`). The hidden `^M` (`\r`) character at the end of the shebang line causes execution failure.

### Solution Workflow:
```bash
# Convert script line endings from CRLF to LF using dos2unix
dos2unix script.sh

# Alternatively, use sed to strip carriage returns
sed -i 's/\r$//' script.sh
```

---

## 🚨 2. Issue: `File Not Found` Errors Due to Case Sensitivity

### Root Cause:
Windows filesystems (NTFS/FAT32) are case-insensitive. A developer referencing `import Config from './config.js'` will succeed on Windows even if the file on disk is named `Config.js`. On Linux, case sensitivity causes a `Module Not Found` runtime crash.

### Solution Workflow:
Enforce case sensitivity checks in Git configuration and code linters:
```bash
# Force Git to respect filename case changes across platforms
git config core.ignorecase false
```

---

## 🚨 3. Issue: Hardcoded Windows File Paths (`C:\path\to\file`)

### Root Cause:
Windows uses backslashes (`\`), while Linux uses forward slashes (`/`).

### Solution:
Always use forward slashes (`/`) or environment path jointers (`path.join()` in Node.js / `os.path.join()` in Python) in cross-platform codebases.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Real World Production Scenarios](./09-Real-World-Production-Scenarios.md) | [README](./README.md) | [11 - Interview QA](./11-Interview-QA.md) |
