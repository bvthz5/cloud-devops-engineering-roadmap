# 07 - Real-World DevOps Scenarios

These nine scenarios demonstrate how grep, sed, and awk are used in genuine daily tasks by system administrators, SREs, and DevOps engineers. For each scenario, the tool choice is explained.

---

## 🏭 Scenario 1 — Find Application Errors

**Task:** Search an application log for error lines. Also show surrounding context when investigating a specific failure.

```bash
# Basic error search
grep -i "error" app.log

# Show 3 lines of context around each error (essential for root cause)
grep -C 3 -i "error" app.log
```

**Why grep?** The task is purely to **find** matching lines. grep's `-C` flag adds context without any additional tools.

---

## 🏭 Scenario 2 — Find TODOs Across a Codebase

**Task:** Find all TODO comments in a source code directory, showing which file and line each appears on.

```bash
grep -rn "TODO" src/
```

**Why grep?** `-r` (recursive) + `-n` (line numbers) makes grep the fastest tool for this pattern search across many files.

---

## 🏭 Scenario 3 — Extract a Specific Column from CSV

**Task:** Extract only the employee names from `employees.txt`.

```bash
awk -F',' 'NR > 1 {print $2}' employees.txt
```

**Why awk?** grep cannot extract a single column. awk's field splitter makes column extraction trivial.

---

## 🏭 Scenario 4 — Find High-Earning Employees

**Task:** Show all employees earning more than $60,000 with their salary.

```bash
awk -F',' 'NR > 1 && $4 > 60000 {print $2, $4}' employees.txt
```

**Why awk?** The task requires both **column extraction** and a **numeric comparison** — awk handles both in one pass.

---

## 🏭 Scenario 5 — Replace an API Endpoint in a Config File

**Task:** Update an old API hostname to a new one across a config file. Use a different delimiter because the value contains slashes.

```bash
# Step 1: TEST — preview the change
sed 's|old-api.internal|new-api.production|g' config.conf

# Step 2: APPLY only after verifying output is correct
sed -i.bak 's|old-api.internal|new-api.production|g' config.conf
```

**Why sed?** sed is designed for stream substitution. Using `|` as delimiter avoids escaping. The `.bak` backup enables rollback.

---

## 🏭 Scenario 6 — Identify Top Client IPs from a Web Access Log

**Task:** Find which IP addresses are making the most requests to your web server.

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head
```

**Pipeline breakdown:**

```text
awk '{print $1}'   → Extract IP addresses (first field per line)
sort               → Sort IPs alphabetically (required for uniq)
uniq -c            → Count consecutive duplicate lines (each unique IP)
sort -rn           → Sort by count descending
head               → Show top 10 results
```

**Why this pipeline?** No single tool can do all steps. The tools complement each other perfectly.

---

## 🏭 Scenario 7 — Clean a Configuration File

**Task:** Strip comment lines and blank lines from a config file to produce a clean, readable version.

```bash
grep -v "^#" config.conf | grep -v "^$" > config-clean.conf
```

**Alternative with sed:**

```bash
sed '/^#/d; /^$/d' config.conf > config-clean.conf
```

**Why grep or sed?** Both work here. grep is slightly more readable for two independent filters; sed is more concise with semicolons.

---

## 🏭 Scenario 8 — Investigate a Failing Service

**Task:** Check recent nginx journal entries for errors.

```bash
journalctl -u nginx | grep -i error
```

**Extend with context:**

```bash
journalctl -u nginx --since "1 hour ago" | grep -C 2 -i "error"
```

**Why grep?** The goal is purely filtering; `journalctl` produces the data, grep filters it.

---

## 🏭 Scenario 9 — Build a Department Headcount Report

**Task:** Count how many employees are in each department and display a formatted header.

```bash
awk -F',' '
BEGIN { print "Department     | Count" }
NR > 1 { count[$3]++ }
END { for (d in count) printf "%-15s| %d\n", d, count[d] }
' employees.txt
```

**Output:**

```text
Department     | Count
Engineering    | 3
Marketing      | 2
HR             | 1
```

**Why awk?** This is a grouped aggregation with formatted output — exactly what awk's associative arrays and `printf` are built for.

---

## 📌 Scenario Summary Table

| # | Task | Primary Tool | Key Technique |
| :---: | :--- | :---: | :--- |
| 1 | Find error lines + context | `grep` | `-C 3 -i` |
| 2 | Search TODOs in code | `grep` | `-rn` |
| 3 | Extract a CSV column | `awk` | `-F',' NR>1 {print $2}` |
| 4 | Filter by numeric condition | `awk` | `$4 > 60000` |
| 5 | Replace config value | `sed` | `-i.bak 's|old|new|g'` |
| 6 | Top IPs in access log | Pipeline | `awk \| sort \| uniq -c \| sort -rn` |
| 7 | Clean config file | `grep` / `sed` | `-v "^#"` + `-v "^$"` |
| 8 | Check service logs | `grep` | `journalctl -u \| grep -i` |
| 9 | Department report | `awk` | `BEGIN{}` + `END{}` + `printf` |
