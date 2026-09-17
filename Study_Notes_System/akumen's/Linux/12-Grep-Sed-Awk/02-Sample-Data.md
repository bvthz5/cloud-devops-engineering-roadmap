# 02 - Sample Data File

All examples throughout this module use a common CSV file called **`employees.txt`**. Understanding its structure is essential for following all the grep, sed, and awk commands.

---

## 📄 File Contents

```csv
ID,Name,Department,Salary
101,Alice,Engineering,75000
102,Bob,Marketing,55000
103,Charlie,Engineering,82000
104,Diana,HR,48000
105,Eve,Engineering,91000
106,Frank,Marketing,52000
```

---

## 🗂️ Column Reference

```text
┌────┬─────────┬─────────────┬────────┐
│ $1 │   $2    │     $3      │   $4   │
├────┼─────────┼─────────────┼────────┤
│ ID │  Name   │ Department  │ Salary │
├────┼─────────┼─────────────┼────────┤
│101 │ Alice   │ Engineering │ 75000  │
│102 │ Bob     │ Marketing   │ 55000  │
│103 │ Charlie │ Engineering │ 82000  │
│104 │ Diana   │ HR          │ 48000  │
│105 │ Eve     │ Engineering │ 91000  │
│106 │ Frank   │ Marketing   │ 52000  │
└────┴─────────┴─────────────┴────────┘

$NF = last field = $4 = Salary
NR  = current record number (1 = header row)
NF  = number of fields = 4
```

---

## ⚠️ Two Critical Facts About This File

### 1. Comma-Separated — Always Set `-F','` in awk

Because fields are separated by commas, **not whitespace**, awk requires the field separator flag:

```bash
awk -F',' '{print $2}' employees.txt
#              ↑
#     Without this, $2 would fail — awk defaults to whitespace
```

### 2. Row 1 is a Header — Use `NR > 1` to Skip It

```bash
awk -F',' 'NR > 1 {print $2, $4}' employees.txt
#           ↑
#     Skips the "ID,Name,Department,Salary" header row
```

Without `NR > 1`, calculations like sum and average would try to add `"Salary"` as a number, producing incorrect results.

---

## 🛠️ Create the File Yourself

Use this heredoc to recreate the file in any lab environment:

```bash
cat > employees.txt <<'EOF'
ID,Name,Department,Salary
101,Alice,Engineering,75000
102,Bob,Marketing,55000
103,Charlie,Engineering,82000
104,Diana,HR,48000
105,Eve,Engineering,91000
106,Frank,Marketing,52000
EOF
```

---

## 🔍 Quick Sanity Checks

```bash
# Verify the file exists and has correct content
cat employees.txt

# Count lines (should be 7 including header)
wc -l employees.txt

# Check column count in a line (should be 4)
awk -F',' '{print NF}' employees.txt | sort -u
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Text Processing Overview](./01-Text-Processing-Overview.md) | [README](./README.md) | [03 - grep Pattern Searching](./03-grep-Pattern-Searching.md) |
