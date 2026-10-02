# 06 - Combining grep, sed, and awk with Pipes

The true power of these tools emerges when they are chained together using the **pipe operator** (`|`). Each command receives the output of the previous command as its input — creating efficient, composable data pipelines without temporary files.

---

## 🔗 How Pipes Work

```text
Command 1
    │  stdout
    │ ──────▶
    ▼
Command 2
    │  stdout
    │ ──────▶
    ▼
Command 3
    │  stdout
    │ ──────▶
    ▼
Final Output (terminal or redirect to file)
```

---

## 📋 Pipeline Examples

### Find Error Lines and Extract Specific Columns

```bash
grep "ERROR" app.log | awk '{print $1, $2, $5}'
# grep filters only ERROR lines
# awk extracts date, time, and error message field
```

### Transform Text Then Filter

```bash
sed 's/WARN/WARNING/g' app.log | grep "WARNING"
# sed normalizes abbreviations first
# grep then searches the consistent spelling
```

### Count Unique Client IPs in a Web Access Log

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head
# awk  → extract IP column
# sort → alphabetically sort IPs
# uniq -c → count consecutive duplicates
# sort -rn → sort by count descending
# head → show top 10
```

### Total Engineering Salaries

```bash
grep "Engineering" employees.txt | awk -F',' '{sum += $4} END {print "Engineering Total:", sum}'
# grep  → filter to Engineering rows only
# awk   → sum the salary column (no NR>1 needed — header won't match grep)
```

### Clean a Config File (Remove Comments and Blank Lines)

```bash
grep -v "^#" httpd.conf | grep -v "^$" > httpd-clean.conf
# First grep  → remove comment lines
# Second grep → remove blank lines
# Output redirected to a cleaned config file
```

### Top 10 Memory-Hungry Processes

```bash
ps aux | awk 'NR > 1 {print $4, $11}' | sort -rn | head -10
# ps aux   → list all processes
# awk      → extract memory % (col 4) and command name (col 11)
# sort -rn → sort by memory descending
# head -10 → show top 10
```

### Anonymize IPv4 Addresses in a Log

```bash
sed -E 's/[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+/XXX.XXX.XXX.XXX/g' access.log
# Extended regex (-E) matches any IPv4 address pattern
# Replaces with a placeholder — useful for sharing sanitized logs
```

---

## 🧠 DevOps Mindset for Tool Selection in Pipelines

```text
┌─────────────────────────────────────────────────────┐
│  Problem                    │  Right Tool            │
├─────────────────────────────┼────────────────────────┤
│  Filter to relevant lines   │  grep                  │
│  Normalize / clean text     │  sed                   │
│  Extract a column           │  awk or cut            │
│  Count occurrences          │  grep -c or uniq -c    │
│  Sort results               │  sort                  │
│  De-duplicate               │  uniq                  │
│  Compute a total/average    │  awk (END block)        │
│  Format a report            │  awk (printf + BEGIN)  │
└─────────────────────────────┴────────────────────────┘
```

---

## 🔬 Debugging Pipelines

When a pipeline gives unexpected output, **debug one command at a time**:

```bash
# Step 1: Verify the first command alone
awk '{print $1}' access.log

# Step 2: Add the second and check again
awk '{print $1}' access.log | sort

# Step 3: Add the third and check
awk '{print $1}' access.log | sort | uniq -c

# Continue until you isolate the problematic command
```

This strategy is described further in [`08-Troubleshooting.md`](./08-Troubleshooting.md).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - awk Fields and Reports](./05-awk-Fields-and-Reports.md) | [README](./README.md) | [07 - Real World DevOps Scenarios](./07-Real-World-DevOps-Scenarios.md) |
