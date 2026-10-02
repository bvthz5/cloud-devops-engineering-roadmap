# 03 - grep — Pattern Searching

**grep** (**G**lobal **R**egular **E**xpression **P**rint) searches text line-by-line and outputs every line that matches a given pattern. It is the first tool to reach for when the goal is simply to **find** something.

---

## 🔤 Basic Syntax

```bash
grep "pattern" file
grep [options] "pattern" file
```

---

## 🔍 Basic Search Examples

```bash
# Find all lines containing "Engineering"
grep "Engineering" employees.txt

# Output:
# 101,Alice,Engineering,75000
# 103,Charlie,Engineering,82000
# 105,Eve,Engineering,91000
```

---

## 🏷️ Common Options Reference

```text
┌──────┬────────────────────────────────────────────────────┐
│ Flag │ Description                                        │
├──────┼────────────────────────────────────────────────────┤
│  -i  │ Case-insensitive match                             │
│  -n  │ Show line numbers for each match                   │
│  -c  │ Count the number of matching lines                 │
│  -v  │ Invert match — show non-matching lines             │
│  -w  │ Whole-word match only                              │
│  -r  │ Recursive search through directories               │
│  -l  │ Show only filenames that contain a match           │
│  -L  │ Show only filenames that do NOT match              │
│  -o  │ Print only the matched portion (not the full line) │
│  -E  │ Extended regular expressions (like egrep)          │
│  -P  │ Perl-compatible regex (if supported by your build) │
│  -e  │ Specify multiple patterns in one command           │
│  -A  │ Show N lines After each match                      │
│  -B  │ Show N lines Before each match                     │
│  -C  │ Show N lines of Context (before + after)           │
└──────┴────────────────────────────────────────────────────┘
```

---

## 📋 Option Examples

### Case-Insensitive Search

```bash
grep -i "alice" employees.txt
# Matches: alice, Alice, ALICE
```

### Show Line Numbers

```bash
grep -n "Marketing" employees.txt
# Output:
# 3:102,Bob,Marketing,55000
# 7:106,Frank,Marketing,52000
```

### Count Matching Lines

```bash
grep -c "Engineering" employees.txt
# Output: 3
```

### Invert Match (Non-Matching Lines)

```bash
grep -v "Engineering" employees.txt
# Shows all lines that are NOT Engineering employees
# (includes the header row)
```

### Whole-Word Match

```bash
grep -w "HR" employees.txt
# Matches "HR" as a whole word — won't accidentally match "CHRON"
```

### Recursive Search

```bash
grep -r "TODO" /home/alice/projects/
# Searches all files in the directory tree
```

### Show Filenames with Matches

```bash
grep -l "ERROR" *.log
# Lists filenames that contain at least one ERROR line

grep -L "ERROR" *.log
# Lists filenames with NO ERROR lines
```

### Extract Only the Matched Portion

```bash
grep -oE "[0-9]+" employees.txt
# Prints only the number matches, one per line (IDs and salaries)
```

---

## 🔭 Context Flags — Surrounding Lines

Essential for log analysis when an error line needs surrounding context:

```bash
grep -A 2 "ERROR" app.log   # 2 lines AFTER the match
grep -B 2 "ERROR" app.log   # 2 lines BEFORE the match
grep -C 2 "ERROR" app.log   # 2 lines BEFORE + 2 lines AFTER
```

---

## 🔣 Regular Expressions in grep

### Extended Regex (`-E`) — Multiple Patterns

```bash
grep -E "Alice|Bob" employees.txt
# Matches lines containing Alice OR Bob

grep -E "^[0-9]{3}," employees.txt
# Matches lines starting with exactly 3 digits followed by a comma
```

### Perl-Compatible Regex (`-P`)

```bash
grep -P "\d{5}" employees.txt
# Matches lines containing any 5-digit sequence (the salaries)
# Note: -P availability depends on your grep build
```

### Multiple Patterns (`-e`)

```bash
grep -e "Alice" -e "Bob" employees.txt
# Equivalent to grep -E "Alice|Bob" — more explicit
```

---

## 🏭 DevOps-Specific Examples

```bash
# Search system logs for errors (case-insensitive)
grep -i error /var/log/syslog

# Find all TODO comments in source code with line numbers
grep -rn "TODO" src/

# Check if nginx is running (common in scripts)
ps aux | grep nginx

# Strip comment and blank lines from a config file
grep -v "^#" config.conf | grep -v "^$"

# Find failed SSH logins
grep "Failed password" /var/log/auth.log

# Check if a service name appears in unit files
grep -r "myservice" /etc/systemd/system/
```

---

## 📊 grep Return Codes (Important for Scripting)

grep sets its exit status — useful for conditionals in Bash scripts:

```text
0  →  At least one match was found
1  →  No match found
2  →  An error occurred (bad pattern, file not found)
```

```bash
# Example: conditional in a script
if grep -q "ERROR" app.log; then
    echo "Errors detected — alerting on-call"
fi
# -q (quiet) suppresses output; only the return code matters
```

---

## ⚠️ Key Distinction

> By default, `grep` outputs **the entire matching line**.  
> With `-o`, it outputs **only the matched portion** (one match per output line).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Sample Data](./02-Sample-Data.md) | [README](./README.md) | [04 - sed Stream Editor](./04-sed-Stream-Editor.md) |
