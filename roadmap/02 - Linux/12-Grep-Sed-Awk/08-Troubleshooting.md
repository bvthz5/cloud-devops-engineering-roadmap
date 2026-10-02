# 08 - Troubleshooting grep, sed, and awk

This file documents the most common mistakes, their causes, and how to diagnose and fix them.

---

## 🐛 grep Returns No Output

**Symptom:** `grep "pattern" file` produces nothing.

**Checklist:**

```text
1. Spelling — is the pattern spelled correctly?
2. Case     — does the file use different casing? Try -i
3. Regex    — does the pattern use special chars that need escaping?
4. File     — is the file path correct? Does the file have content?
5. Encoding — is the file UTF-16 or has Windows line endings (\r\n)?
```

**Debug steps:**

```bash
# Check file exists and has content
wc -l employees.txt
cat employees.txt

# Try case-insensitive
grep -i "pattern" file

# Add line numbers to confirm matches
grep -n "pattern" file
```

---

## 🐛 grep Matches Too Many Lines

**Symptom:** grep returns more lines than expected.

**Fix:** Make the pattern more specific — use:

```bash
# Whole-word match (prevents partial matches)
grep -w "HR" employees.txt
# Matches "HR" alone, not "CHRON" or "CHART"

# Use anchors in the pattern
grep "^101," employees.txt    # Must start with "101,"
grep ",75000$" employees.txt  # Must end with ",75000"
```

---

## 🐛 sed Makes Unwanted Changes

**Symptom:** `sed -i` changes text you did not intend to change.

**Root Cause:** Pattern is too broad, or you forgot to test first.

**Prevention workflow:**

```bash
# Always test WITHOUT -i first
sed 's/old/new/g' file.txt

# Review the output carefully
# Only then apply with a backup
sed -i.bak 's/old/new/g' file.txt
```

**Recovery:**

```bash
# If you have a .bak file
cp file.txt.bak file.txt
```

---

## 🐛 sed Path Substitution Fails (Delimiter Conflict)

**Symptom:** `sed 's//old/path/new/path/g'` produces a "too many delimiters" or "unterminated s" error.

**Cause:** The `/` delimiter collides with slashes in the path.

**Fix:** Use a different delimiter:

```bash
# Bad — forward slashes collide
sed 's//home/alice//home/bob/g' config.txt

# Good — pipe as delimiter
sed 's|/home/alice|/home/bob|g' config.txt
```

---

## 🐛 awk Prints the Wrong Column

**Symptom:** `awk '{print $2}'` returns unexpected data.

**Cause:** Mismatch between actual delimiter and awk's default (whitespace).

**Fix:** Set the correct field separator:

```bash
# For CSV (comma-separated)
awk -F',' '{print $2}' employees.txt

# For TSV (tab-separated)
awk -F'\t' '{print $2}' data.tsv

# For colon-separated (/etc/passwd)
awk -F':' '{print $1}' /etc/passwd
```

---

## 🐛 awk Calculation is Wrong (Includes Header)

**Symptom:** Sum or average gives a bizarre or incorrect number.

**Cause:** The header row (`ID,Name,Department,Salary`) is being included. awk tries to add `"Salary"` as a number, treating it as `0`, which silently corrupts the count.

**Fix:** Always add `NR > 1` when calculating over CSV data:

```bash
# Wrong — includes header, count is off
awk -F',' '{sum += $4; count++} END {print sum/count}' employees.txt

# Correct — skip header row
awk -F',' 'NR > 1 {sum += $4; count++} END {print "Average:", sum/count}' employees.txt
```

**Debug step — print the field before calculating:**

```bash
awk -F',' '{print $4}' employees.txt
# First line should print "Salary" — confirming you need NR > 1
```

---

## 🐛 Pipeline Produces Unexpected Output

**Strategy:** Debug one command at a time.

```bash
# Test command 1 alone
awk '{print $1}' access.log

# Test commands 1 + 2
awk '{print $1}' access.log | sort

# Test commands 1 + 2 + 3
awk '{print $1}' access.log | sort | uniq -c

# Add next command only when current output looks correct
```

Isolate the step where the data changes unexpectedly — that is the broken command.

---

## ⚠️ Critical Safety Rule

> **Do NOT use `sed -i` on production files without:**
> 1. Testing the transformation without `-i` first
> 2. Taking a backup with `sed -i.bak` or `cp file.bak file`
> 3. Having a rollback plan if something goes wrong

---

## 📋 Troubleshooting Quick Reference

| Symptom | Likely Cause | Fix |
| :--- | :--- | :--- |
| grep no output | Wrong case or spelling | Add `-i`; check spelling |
| grep too many matches | Pattern too broad | Use `-w` or anchors |
| sed changes wrong text | Pattern too broad | Test without `-i` first |
| sed delimiter error | `/` in pattern/path | Use `s\|old\|new\|g` |
| awk wrong column | Wrong FS | Add `-F','` or correct separator |
| awk wrong calculation | Header included | Add `NR > 1` condition |
| Pipeline wrong output | One bad step | Test commands one at a time |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real World DevOps Scenarios](./07-Real-World-DevOps-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
