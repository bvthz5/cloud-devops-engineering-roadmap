# 09 - Interview Questions and Answers

23 technical interview questions covering grep, sed, and awk — from fundamentals to applied scenarios.

---

## 🟢 Fundamentals

**Q1. What is grep and what does it do?**
> grep (Global Regular Expression Print) is a command-line tool that searches text line by line for lines matching a given pattern and outputs those lines.

---

**Q2. What is sed?**
> sed (Stream Editor) is a non-interactive command-line tool that reads input line by line and applies editing commands — most commonly text substitution, line deletion, insertion, and printing.

---

**Q3. What is awk?**
> awk is a pattern-scanning and processing language/tool that treats input as records split into fields. It is best suited for structured text processing — extracting columns, filtering rows by conditions, performing calculations, and producing formatted reports.

---

**Q4. How do grep, sed, and awk differ in purpose?**
> - **grep** → FIND matching lines
> - **sed** → CHANGE/TRANSFORM text
> - **awk** → ANALYZE structured data, compute, and report

---

## 🔵 grep Questions

**Q5. How do you search case-insensitively with grep?**
```bash
grep -i "pattern" file
```

---

**Q6. How do you search recursively through directories?**
```bash
grep -r "pattern" /path/to/directory/
```

---

**Q7. How do you show line numbers alongside grep matches?**
```bash
grep -n "pattern" file
```

---

**Q8. How do you count matching lines instead of showing them?**
```bash
grep -c "pattern" file
```

---

**Q9. How do you show only lines that do NOT match?**
```bash
grep -v "pattern" file
```

---

**Q10. How do you show the filename and 2 lines of context around each match?**
```bash
grep -rn -C 2 "ERROR" *.log
```

---

**Q11. What are grep's return codes and why do they matter?**
> - `0` → match found
> - `1` → no match found
> - `2` → error (bad regex, missing file)
>
> Return codes enable grep to be used in Bash conditionals:
> ```bash
> if grep -q "ERROR" app.log; then echo "Alert!"; fi
> ```

---

## 🟠 sed Questions

**Q12. How do you substitute text with sed?**
```bash
# Replace first occurrence per line
sed 's/old/new/' file

# Replace ALL occurrences per line (global)
sed 's/old/new/g' file
```

---

**Q13. How do you edit a file in-place with sed?**
```bash
sed -i 's/old/new/g' file
```
> ⚠️ **Always test without `-i` first.** Use `-i.bak` to keep a backup.

---

**Q14. Why is `sed -i` considered risky?**
> It modifies the original file directly with no confirmation. If the pattern is wrong, data can be corrupted. Always preview the output without `-i` before applying it.

---

**Q15. How do you use sed to keep a backup while editing in place?**
```bash
sed -i.bak 's/old/new/g' file
# Creates file.bak as a backup before modifying file
```

---

**Q16. How do you remove all comment lines and blank lines from a config with sed?**
```bash
sed '/^#/d; /^$/d' config.conf
```

---

**Q17. How do you handle path substitution in sed without delimiter errors?**
```bash
# Use | as an alternative delimiter
sed 's|/old/path|/new/path|g' file
```

---

## 🟣 awk Questions

**Q18. What does `$1` mean in awk?**
> `$1` refers to the first field (column) of the current record. Fields are delimited by the field separator (default: whitespace).

---

**Q19. What does `$0` represent?**
> `$0` represents the entire current line — all fields combined.

---

**Q20. What is `$NF`?**
> `$NF` is the last field in the current record. `NF` (Number of Fields) varies per line; `$NF` always refers to whichever field is last.

---

**Q21. What are `NR` and `NF`?**
> - `NR` = Number of Records processed so far = current line number
> - `NF` = Number of Fields in the current record

---

**Q22. What do `BEGIN` and `END` do in awk?**
> - `BEGIN` runs **once before** any input is read — used for headers, initialization
> - `END` runs **once after** all input is processed — used for totals, reports, summaries

---

**Q23. Show a complete pipeline: find errors and extract the timestamp and error message.**
```bash
grep "ERROR" app.log | awk '{print $1, $2, $5}'
# grep filters to ERROR lines
# awk extracts date ($1), time ($2), and message field ($5)
```

---

## 💡 30-Second Interview Answer

> *"grep, sed, and awk are core Linux text-processing tools. grep searches for matching patterns, sed transforms streams of text through substitution and line operations, and awk processes structured fields — performing filtering, calculations, and report generation. They become especially powerful when combined with pipes to analyze logs, configuration files, command output, and structured data — all without writing full scripts."*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands On Lab](./10-Hands-On-Lab.md) |
