# 10 — Text Processing Pipelines and Data Wrangling

The true superpower of a DevOps engineer is the ability to parse gigabytes of server logs, CSV files, and API outputs directly from the command line using standard POSIX text-processing tools.

---

## 1. The Core Text-Processing Toolkit

```text
+-------------------------------------------------------------+
|                Unix Text Processing Arsenal                 |
+-------------------------------------------------------------+
| Tool    | Primary Responsibility                            |
+---------+---------------------------------------------------+
| grep    | Fast line searching via regular expressions       |
| awk     | Column-based pattern scanning and arithmetic data |
| sed     | Stream editor for non-interactive text replacement|
| cut     | Simple delimiter-based column extraction          |
| sort    | Alphabetical, numeric, and keyed line sorting     |
| uniq    | Duplicate detection and frequency aggregation     |
| tr      | Character-by-character translation and deletion   |
| wc      | Line, word, and byte counting                     |
+-------------------------------------------------------------+
```

---

## 2. Searching Text with `grep`

**`grep`** (Global Regular Expression Print) processes text line-by-line, printing lines matching a search pattern:

```bash
# 1. Case-insensitive search
grep -i "error" /var/log/syslog

# 2. Invert match: Print all lines that DO NOT contain "healthcheck"
grep -v "healthcheck" /var/log/nginx/access.log

# 3. Recursive directory search with line numbers
grep -rn "DATABASE_URL" /etc/systemd/system/

# 4. Extended Regular Expressions (grep -E / egrep)
# Search for HTTP status codes 500, 502, 503, or 504:
grep -E "HTTP/1.1\" 50[0-4]" access.log

# 5. Context flags: Print 3 lines before (-B) and after (-A) the match
grep -B 2 -A 3 "FATAL EXCEPTION" app.log
```

---

## 3. Stream Editing with `sed`

**`sed`** is a non-interactive stream editor used to transform, substitute, and delete text streams:

```bash
# 1. Basic Substitution: Replace first occurrence of "http://" with "https://"
sed 's/http:\/\//https:\/\//' urls.txt

# 2. Global Substitution (all occurrences on every line):
sed 's/localhost/127.0.0.1/g' config.yaml

# 3. In-Place File Editing (-i): Overwrites the actual file on disk
sed -i 's/DEBUG=True/DEBUG=False/g' .env

# 4. Delete lines matching a pattern (e.g., remove all comment lines starting with #)
sed -i '/^[[:space:]]*#/d' nginx.conf

# 5. Delete empty lines
sed -i '/^$/d' data.txt
```

---

## 4. Column Data Extraction: `cut` vs `awk`

### `cut` (Simple, Fast Delimited Splitting)
Use `cut` when records are separated by a single consistent delimiter (like `:` or `,`):
```bash
# Extract username (field 1) and shell (field 7) from /etc/passwd
cut -d: -f1,7 /etc/passwd

# Extract columns 1 through 4 from CSV file
cut -d, -f1-4 data.csv
```

### `awk` (Full Programming Language for Columns & Math)
Use `awk` when fields are separated by variable whitespace (spaces and tabs), or when calculations/conditions are required:
```bash
# 1. Print columns 1 and 9 of 'ls -l' (ignores multiple spaces!)
ls -l /var/log | awk '{print $1, $9}'

# 2. Conditional filtering: Print processes using > 10% CPU
ps aux | awk '$3 > 10.0 {print $2, $3, $11}'

# 3. Arithmetic aggregation: Sum total RAM consumed by all Nginx workers in MB
ps -C nginx -o rss= | awk '{sum += $1} END {printf "Total Nginx RAM: %.2f MB\n", sum/1024}'
```

---

## 5. Sorting and Frequency Aggregation: `sort` and `uniq`

> **CRITICAL RULE:** `uniq` **only detects duplicate lines that are immediately adjacent**. Therefore, you **MUST sort the data before piping it into `uniq`!**

```bash
# 1. Sort lines alphabetically
sort names.txt

# 2. Sort lines numerically in reverse order (highest to lowest)
sort -nr numbers.txt

# 3. Count unique occurrences (frequency distribution)
sort input.txt | uniq -c | sort -nr
```

---

## 6. Real-World Pipeline Recipes for DevOps & SREs

### Recipe 1: Top 10 IP Addresses Hitting an Nginx/Apache Web Server
```bash
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -n 10
```
Output:
```text
  14502 192.168.1.105
   8912 10.0.4.12
   3100 172.16.0.40
```

### Recipe 2: Identify Top 5 Memory-Consuming Processes
```bash
ps aux --sort=-%mem | awk 'NR<=6 {printf "%-10s %-8s %-6s %s\n", $1, $2, $4, $11}'
```

### Recipe 3: Find All Failed SSH Login Attempts (Audit Intrusion Detection)
```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
```

### Recipe 4: Count Total Number of Active TCP Connections by State
```bash
ss -tan | awk 'NR>1 {print $1}' | sort | uniq -c | sort -nr
```
Output:
```text
    451 ESTAB
     32 TIME-WAIT
      8 LISTEN
      2 CLOSE-WAIT
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Help Systems and Introspection Tools](./09-Help-Systems-and-Introspection-Tools.md) | [README](./README.md) | [11 - Shell Scripting Foundations and Defensive Bash](./11-Shell-Scripting-Foundations-and-Defensive-Bash.md) |
