# 12 - Quick Revision Cheat Sheet

High-density 5-minute summary for fast review before interviews, exams, or on-call tasks.

---

## 🔑 The Trio at a Glance

| Tool | Job | One-Line Answer |
| :---: | :--- | :--- |
| `grep` | **Find** | "Which lines match this pattern?" |
| `sed` | **Change** | "Replace/delete/insert this text" |
| `awk` | **Analyze** | "Extract columns, compute, report" |

> **Golden Rule:** Find → grep · Change → sed · Analyze → awk

---

## 🔍 grep One-Liners

```text
grep "pattern" file          → Basic search
grep -i "pattern" file       → Case-insensitive
grep -n "pattern" file       → Show line numbers
grep -c "pattern" file       → Count matching lines
grep -v "pattern" file       → Invert match (non-matching lines)
grep -w "pattern" file       → Whole-word match only
grep -r "pattern" dir/       → Recursive search
grep -l "pattern" *.log      → Show only matching filenames
grep -o "pattern" file       → Print only matched portion
grep -E "A|B" file           → Extended regex (OR match)
grep -A 2 "ERROR" file       → 2 lines After each match
grep -B 2 "ERROR" file       → 2 lines Before each match
grep -C 2 "ERROR" file       → 2 lines Context (before+after)
grep -q "pattern" file       → Quiet mode (return code only)
```

---

## ✂️ sed One-Liners

```text
sed 's/old/new/' file        → Replace first occurrence per line
sed 's/old/new/g' file       → Replace ALL occurrences per line
sed 's/old/new/gi' file      → Case-insensitive global replace
sed -i 's/old/new/g' file    → Edit file in-place (test first!)
sed -i.bak 's/old/new/g' f  → In-place with .bak backup
sed 's|/old|/new|g' file     → Use | as delimiter (for paths)
sed -n '3p' file             → Print line 3 only
sed -n '2,5p' file           → Print lines 2-5
sed -n '/pattern/p' file     → Print matching lines only
sed '1d' file                → Delete line 1 (header)
sed '2,4d' file              → Delete lines 2-4
sed '/pattern/d' file        → Delete matching lines
sed '/^#/d' file             → Delete comment lines
sed '/^$/d' file             → Delete blank lines
sed '2i\text' file           → Insert text BEFORE line 2
sed '2a\text' file           → Append text AFTER line 2
sed -e 's/a/b/g' -e 's/c/d/g' f → Multiple substitutions
```

---

## 🧮 awk One-Liners

```text
awk '{print $1}' file              → Print first column (whitespace FS)
awk -F',' '{print $2}' file        → Print second column (CSV)
awk -F',' '{print $2, $4}' file    → Print multiple columns
awk -F',' 'NR > 1 {print $2}' file → Skip header row
awk -F',' '$4 > 60000 {print $2}' → Filter by numeric value
awk -F',' '$3 == "HR" {print $2}' → Filter by string equality
awk -F',' '/Engineering/ {print $2}' → Filter by regex pattern
awk -F',' 'NR > 1 {sum += $4} END {print sum}' file   → Sum column
awk -F',' 'NR > 1 {sum += $4; count++} END {print sum/count}' → Average
awk -F',' 'NR > 1 {count[$3]++} END {for (d in count) print d, count[d]}' → Group count
awk -F',' 'NR > 1 {printf "%-10s %8d\n", $2, $4}' → Formatted output
awk 'BEGIN {OFS="|"} {print $1,$2}' → Custom output separator
```

---

## 🔢 awk Variables Pocket Reference

```text
$0   → Whole line
$1   → First field
$2   → Second field
$NF  → Last field
NR   → Current line/record number
NF   → Number of fields in current record
FS   → Input field separator  (set with -F or in BEGIN)
OFS  → Output field separator (set in BEGIN block)
```

---

## 🔗 Pipeline One-Liners

```text
grep "ERROR" app.log | awk '{print $1, $2}'       → Filter then extract
sed 's/WARN/WARNING/g' log | grep "WARNING"        → Transform then filter
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head  → Top IPs
grep "Engineering" employees.txt | awk -F',' '{sum+=$4} END{print sum}'
grep -v "^#" config.conf | grep -v "^$"           → Remove comments+blanks
ps aux | awk 'NR>1 {print $4, $11}' | sort -rn | head -10  → Top memory
```

---

## 💡 Must-Know Safety Rules

| Rule | Detail |
| :--- | :--- |
| Always test `sed` without `-i` first | Preview output before modifying the file |
| Use `-i.bak` for backups | `sed -i.bak 's/old/new/g' file` |
| Add `NR > 1` for CSV headers | Prevents header row from corrupting calculations |
| Use alternative delimiters for paths | `sed 's\|/old\|/new\|g'` avoids errors |
| Debug pipelines one step at a time | Add commands one by one to isolate failures |

---

## 🎯 30-Second Interview Answer

> *"grep, sed, and awk are core Linux text-processing tools. grep searches for matching patterns, sed transforms text through substitution and line operations, and awk processes structured fields — performing filtering, calculations, and report generation. They become especially powerful when combined with pipes to analyze logs, configs, and structured data without writing full scripts."*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQs](./11-MCQs.md) | [README](./README.md) | [13 - Related Topics](./13-Related-Topics.md) |
