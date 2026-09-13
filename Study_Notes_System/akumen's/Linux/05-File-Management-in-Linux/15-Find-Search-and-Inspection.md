# 15 - Search & File Inspection: `find`, `grep`, and `file`

Locating specific files across large filesystems and filtering text within logs are essential daily workflows for SREs and system administrators.

---

## 1. `find` — Directory Tree Search Engine

`find` recursively scans directory trees evaluating criteria in real time.

```bash
# Syntax: find <starting_directory> <matching_criteria> <action>
```

### High-Power `find` Recipes:

```bash
# 1. Search by filename (case-insensitive):
$ find /var/log -iname "*nginx*"

# 2. Search by file type (-type f=file, d=directory, l=symlink):
$ find /etc -type d -name "conf.d"

# 3. Search by file size (+ greater than, - less than):
$ sudo find / -type f -size +500M 2>/dev/null

# 4. Search by modification time (-mtime in days, -mmin in minutes):
# Find log files modified in the last 24 hours:
$ find /var/log -type f -name "*.log" -mtime -1

# 5. Search by permissions (e.g. Find world-writable files for security audit):
$ find /data -type f -perm -0002 2>/dev/null

# 6. Execute actions on matched files (-exec ... {} +):
# Find files older than 30 days and compress them:
$ find /var/log/app -name "*.log" -mtime +30 -exec gzip {} +

# 7. Delete matched files safely:
$ find /tmp -type f -name "*.tmp" -delete
```

---

## 2. `grep` — Text Pattern Search Engine

`grep` (Global Regular Expression Print) searches inside file contents for matching text patterns.

### Essential `grep` Flags:

| Flag | Function | Example |
|:---:|---|---|
| **`-i`** | Case-insensitive search | `grep -i "error" app.log` |
| **`-r` / `-R`**| Recursive search inside directory | `grep -r "DATABASE_URL" /etc/` |
| **`-n`** | Show line numbers | `grep -n "FATAL" server.log` |
| **`-v`** | Invert match (exclude lines containing pattern) | `grep -v "#" /etc/nginx/nginx.conf` |
| **`-w`** | Match exact whole word only | `grep -w "user" /etc/passwd` |
| **`-c`** | Count matching lines | `grep -c "404" access.log` |
| **`-l`** | Print only filenames containing matches | `grep -rl "secret_key" /opt/` |
| **`-E`** | Extended Regular Expressions (regex) | `grep -E "404|500|502" access.log` |

```bash
# Production Clean Config Viewer (Strips comments '#' and empty lines):
$ grep -Ev "^#|^$" /etc/nginx/nginx.conf
```

---

## 3. `file` — Inspecting File Types via Magic Bytes

Linux does not trust file extensions. An attacker can name a binary executable `image.png`. The `file` command reads the **header magic bytes** of the file to report its true MIME format:

```bash
$ file /bin/bash
/bin/bash: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked

$ file logo.png
logo.png: PNG image data, 800 x 600, 8-bit/color RGBA, non-interlaced

$ file fake_image.png
fake_image.png: POSIX shell script, ASCII text executable  <-- SECURITY ALERT!
```
