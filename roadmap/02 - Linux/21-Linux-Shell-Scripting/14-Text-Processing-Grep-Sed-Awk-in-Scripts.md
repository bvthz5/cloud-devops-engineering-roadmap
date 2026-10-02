# 14 — Text Processing: Grep, Sed, and Awk in Shell Scripts

## 1. Overview of Text Processing Utilities in Shell Pipelines

Shell scripting excels at automating system administration by chaining text processing utilities (`grep`, `sed`, `awk`, `cut`, `tr`, `sort`, `uniq`) into powerful shell pipelines.

```
+------------------+     stdout     +------------------+     stdout     +------------------+
|    Raw Log /     | -------------> |   grep Filter    | -------------> |    sed Replace   |
|   Input Stream   |                | (Select Matching)|                | (Modify Pattern) |
+------------------+                +------------------+                +------------------+
                                                                                 |
                                                                              stdout
                                                                                 v
+------------------+     stdout     +------------------+                +------------------+
|  Structured Data | <------------- |    sort / uniq   | <------------- |   awk Formatter  |
|  / Report Output |                | (Aggregate/Count)|     stdout     | (Extract Columns)|
+------------------+                +------------------+                +------------------+
```

---

## 2. Using `grep` (Global Regular Expression Print) in Scripts

`grep` searches input files or streams for lines matching a pattern and returns exit status `0` if a match is found, or `1` if no match is found.

### Key `grep` Flags for DevOps Scripts

| Flag | Meaning | Example Use Case |
|---|---|---|
| `-q` | Quiet mode. Suppresses stdout/stderr output. Returns exit status only. | `if grep -q "ERROR" app.log; then ...` |
| `-i` | Case-insensitive matching. | `grep -i "failed" syslog` |
| `-v` | Invert match (select non-matching lines). | `grep -v "^#" config.conf` (Strip comments) |
| `-E` | Extended Regular Expressions (ERE). Enables `|`, `+`, `?`. | `grep -E "404|500" access.log` |
| `-c` | Count matching lines. | `ERR_COUNT=$(grep -c "FATAL" app.log)` |

### Practical Example: Validating Active Users in `/etc/passwd`
```bash
#!/usr/bin/env bash
set -euo pipefail

TARGET_USER="ubuntu"

if grep -q -E "^${TARGET_USER}:" /etc/passwd; then
  echo "User '$TARGET_USER' exists on the system."
else
  echo "User '$TARGET_USER' does NOT exist." >&2
  exit 1
fi
```

---

## 3. Using `sed` (Stream Editor) in Scripts

`sed` performs line-by-line text transformation, substitution, deletion, and insertion on standard input or files.

### Key `sed` Operations

```bash
# 1. Substitute first occurrence of 'OLD' with 'NEW' per line:
sed 's/OLD/NEW/' input.txt

# 2. Substitute ALL occurrences of 'OLD' with 'NEW' globally per line:
sed 's/OLD/NEW/g' input.txt

# 3. In-place file modification (-i):
sed -i 's/listen 80;/listen 8080;/g' /etc/nginx/conf.d/app.conf

# 4. Delete lines matching pattern:
sed -i '/^#/d' /etc/config.conf      # Remove comment lines
sed -i '/^$/d' /etc/config.conf      # Remove empty lines

# 5. Extract specific lines (e.g. lines 10 to 20):
sed -n '10,20p' /var/log/syslog
```

---

## 4. Using `awk` (Pattern Scanning and Processing Language) in Scripts

`awk` operates on structured tabular data, processing inputs line by line and splitting fields automatically by whitespace or custom delimiters.

### Builtin `awk` Variables

| Variable | Description |
|---|---|
| `$0` | The entire current record (line). |
| `$1, $2, $N` | Field 1, Field 2, Field N of current line. |
| `NF` | Number of fields in current line (`$NF` refers to last field!). |
| `NR` | Current Record Number (line number). |
| `FS` | Input Field Separator (default space/tab). Set via `-F`. |

### Practical `awk` Examples in Shell Scripts

```bash
# 1. Extract memory utilization percentage:
FREE_MEM_PCT=$(free | awk '/Mem:/ {printf "%.2f", ($3/$2)*100}')
echo "Memory Used: ${FREE_MEM_PCT}%"

# 2. Extract IP addresses from Nginx access log and count requests:
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -n 5

# 3. Process CSV file with comma delimiter (-F","):
awk -F"," '$3 == "ACTIVE" {print "User: " $1 ", Email: " $2}' users.csv
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - Functions & Arrays](./13-Functions-Scope-and-Arrays.md) | [README](./README.md) | [15 - Safe File Ops & Locking](./15-Safe-File-Operations-Mktemp-and-Locking.md) |
