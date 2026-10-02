# 05 - awk — Fields, Filtering, and Reports

**awk** is a complete pattern-scanning and processing language. It treats each line of input as a **record** and each space- or delimiter-separated chunk as a **field**. This makes it the ideal tool for working with structured text — CSV/TSV files, logs with fixed columns, and command output.

---

## 🔤 Basic Syntax

```bash
awk 'pattern { action }' file
awk -F',' 'pattern { action }' file   # With field separator
```

If a `pattern` matches, the `action` runs. If no pattern is given, the action runs for every line.

---

## 🔢 Built-In Variables Reference

```text
┌──────────┬──────────────────────────────────────────┐
│ Variable │ Meaning                                  │
├──────────┼──────────────────────────────────────────┤
│   $0     │ The entire current line (all fields)     │
│   $1     │ First field                              │
│   $2     │ Second field                             │
│   $NF    │ Last field (regardless of field count)   │
│   NR     │ Current record (line) number             │
│   NF     │ Number of fields in the current record   │
│   FS     │ Input field separator (default: space)   │
│   OFS    │ Output field separator (default: space)  │
└──────────┴──────────────────────────────────────────┘
```

---

## 📌 Printing Columns

### Single Column (Whitespace-Separated Input)

```bash
awk '{print $1}' file.txt
```

### Single Column (CSV Input — Must Set `-F`)

```bash
awk -F',' '{print $2}' employees.txt
# Prints: Name, Alice, Bob, Charlie, Diana, Eve, Frank
```

### Multiple Columns

```bash
awk -F',' '{print $2, $4}' employees.txt
# Prints Name and Salary separated by a space
```

### Custom Formatting with Literals

```bash
awk -F',' '{print "Name: " $2 " | Dept: " $3}' employees.txt
```

---

## 🚫 Skipping the Header Row (`NR > 1`)

```bash
awk -F',' 'NR > 1 {print $2, $4}' employees.txt
# Skips line 1 (the header) and prints Name + Salary for data rows
```

---

## 🔍 Filtering Rows

### Numeric Comparison

```bash
# Employees earning more than 60,000
awk -F',' 'NR > 1 && $4 > 60000 {print $2, $4}' employees.txt
```

### String Equality

```bash
# Only Engineering employees
awk -F',' '$3 == "Engineering" {print $2, $4}' employees.txt
```

### Regex Pattern Match

```bash
# Lines where any field matches "Engineering"
awk -F',' '/Engineering/ {print $2}' employees.txt
```

---

## 🏗️ BEGIN and END Blocks

```text
BEGIN  →  Runs ONCE before any records are processed
{...}  →  Runs for EACH record that matches the pattern
END    →  Runs ONCE after ALL records have been processed
```

```bash
awk -F',' '
BEGIN { print "=== Salary Report ===" }
NR > 1 { print $2 ": $" $4 }
END   { print "=== End of Report ===" }
' employees.txt
```

---

## ➕ Calculations

### Sum

```bash
awk -F',' 'NR > 1 {sum += $4} END {print "Total Salary:", sum}' employees.txt
```

### Average

```bash
awk -F',' 'NR > 1 {sum += $4; count++} END {print "Average:", sum/count}' employees.txt
```

### Maximum Value (with Name)

```bash
awk -F',' 'NR > 1 && $4 > max {max = $4; name = $2} END {print "Highest Paid:", name, max}' employees.txt
```

---

## 📊 Group and Count (Associative Arrays)

awk has built-in associative arrays — keys can be strings:

```bash
# Count employees per department
awk -F',' 'NR > 1 {count[$3]++} END {for (dept in count) print dept, count[dept]}' employees.txt

# Output (order may vary):
# Engineering 3
# Marketing 2
# HR 1
```

---

## 🖨️ Formatted Output with printf

```bash
# Left-aligned name in 10 chars, right-aligned salary in 8 chars
awk -F',' 'NR > 1 {printf "%-10s %8d\n", $2, $4}' employees.txt

# Output:
# Alice          75000
# Bob            55000
# Charlie        82000
```

---

## 🔗 Custom Output Field Separator (OFS)

```bash
awk -F',' 'BEGIN {OFS=" | "} NR > 1 {print $2, $3, $4}' employees.txt

# Output:
# Alice | Engineering | 75000
# Bob | Marketing | 55000
```

---

## 🏭 DevOps Use Cases

```text
┌──────────────────────────────────────────────────────────────┐
│ Task                               │ Approach                 │
├──────────────────────────────────────────────────────────────┤
│ Analyze CSV / TSV reports          │ awk -F',' '{print $2}'  │
│ Summarize log files                │ awk '{count[$5]++}...'  │
│ Count HTTP requests per IP         │ awk '{print $1}' | sort  │
│ Calculate disk usage totals        │ df -h | awk '{sum+=$4}'  │
│ Extract columns from ps output     │ ps aux | awk '{print $2}'│
│ Format reports with headers        │ BEGIN{} ... END{}        │
│ Filter high-value records          │ $4 > threshold           │
└──────────────────────────────────────────────────────────────┘
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - sed Stream Editor](./04-sed-Stream-Editor.md) | [README](./README.md) | [06 - Combining Pipelines](./06-Combining-Pipelines.md) |
