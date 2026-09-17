# 10 - Hands-On Lab

Work through all four levels in order. Each level builds on the previous. Document every command you run and your explanation of why you chose each tool.

---

## 🛠️ Lab Setup — Create the Sample File

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

Verify:

```bash
cat employees.txt
wc -l employees.txt   # Should be 7 (header + 6 data rows)
```

---

## ⬛ Level 1 — grep Exercises

### Tasks

1. Find all Engineering employees.
2. Search for the name "alice" case-insensitively.
3. Show line numbers for all Marketing rows.
4. Count the number of Engineering rows.
5. Show all employees who are NOT in Engineering.

### Commands to Practice

```bash
# Task 1
grep "Engineering" employees.txt

# Task 2
grep -i "alice" employees.txt

# Task 3
grep -n "Marketing" employees.txt

# Task 4
grep -c "Engineering" employees.txt

# Task 5
grep -v "Engineering" employees.txt
```

### ✅ Verify

```bash
# Task 1 should return 3 lines (Alice, Charlie, Eve)
# Task 4 should print: 3
# Task 5 should return header + Bob, Diana, Frank
```

---

## 🟫 Level 2 — sed Exercises

### Tasks

1. Replace "Engineering" with "Tech" and view the result (do NOT use `-i`).
2. Print only lines 2 through 4.
3. Delete all Marketing rows from output.
4. Delete all blank lines (test with a file that has blanks).

### Commands to Practice

```bash
# Task 1 — preview only
sed 's/Engineering/Tech/g' employees.txt

# Task 2
sed -n '2,4p' employees.txt

# Task 3
sed '/Marketing/d' employees.txt

# Task 4 — create a file with blanks first
printf "line1\n\nline2\n\n\nline3\n" > test.txt
sed '/^$/d' test.txt
```

> 💡 **Rule:** Do not use `-i` in this level until you can confirm the output is correct.

---

## 🟦 Level 3 — awk Exercises

### Tasks

1. Print only the Name column.
2. Print Name and Salary (skip the header).
3. Print employees earning more than $60,000 (skip header).
4. Calculate the total salary of all employees.
5. Calculate the average salary.

### Commands to Practice

```bash
# Task 1
awk -F',' '{print $2}' employees.txt

# Task 2
awk -F',' 'NR > 1 {print $2, $4}' employees.txt

# Task 3
awk -F',' 'NR > 1 && $4 > 60000 {print $2, $4}' employees.txt

# Task 4
awk -F',' 'NR > 1 {sum += $4} END {print "Total:", sum}' employees.txt

# Task 5
awk -F',' 'NR > 1 {sum += $4; count++} END {print "Average:", sum/count}' employees.txt
```

### ✅ Expected Results

```text
Task 4 → Total: 403000
Task 5 → Average: 67166.7
```

---

## 🟥 Level 4 — Pipeline Exercises

### Tasks

1. Find the total salary of Engineering employees only (combine grep + awk).
2. List all department names — unique, sorted.

```bash
# Task 1
grep "Engineering" employees.txt | awk -F',' '{sum += $4} END {print "Engineering Total:", sum}'

# Task 2
awk -F',' 'NR > 1 {print $3}' employees.txt | sort | uniq
```

---

## 🏆 Challenge Scenario

You are given a server log (`/var/log/app.log`) containing timestamps, log levels (INFO, WARN, ERROR), IP addresses, and request information.

**Tasks:**

1. Find all ERROR lines.
2. Show line numbers for each ERROR.
3. Show 3 lines of context around each ERROR.
4. Extract the IP address column (assume it is field `$5`).
5. Count ERROR requests by IP address.
6. Replace `staging.internal` hostname with `prod.internal` in a config file (safely, with a backup).
7. Produce a summary report: count of log levels (INFO, WARN, ERROR).

**Template commands to work from:**

```bash
# 1. Find ERROR lines
grep "ERROR" /var/log/app.log

# 2. With line numbers
grep -n "ERROR" /var/log/app.log

# 3. With context
grep -C 3 "ERROR" /var/log/app.log

# 4. Extract IP (adjust field number as needed)
grep "ERROR" /var/log/app.log | awk '{print $5}'

# 5. Count by IP
grep "ERROR" /var/log/app.log | awk '{print $5}' | sort | uniq -c | sort -rn

# 6. Replace hostname (with backup)
sed -i.bak 's|staging.internal|prod.internal|g' config.conf

# 7. Log level summary report
awk '{print $3}' /var/log/app.log | sort | uniq -c | sort -rn
# (adjust field number $3 to match your actual log format)
```

### 📝 Documentation Task

For each command you run in this challenge:
- Write the command
- Explain **why** you chose grep, sed, or awk
- Explain what the flags do
- Describe what the output means

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQs](./11-MCQs.md) |
