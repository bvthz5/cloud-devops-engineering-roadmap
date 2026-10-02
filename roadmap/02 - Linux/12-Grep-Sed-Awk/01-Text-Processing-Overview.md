# 01 - The Text-Processing Trio: Overview

Linux operates in a world of text. Every log line, configuration directive, process record, and command response is a stream of characters. The ability to search, transform, and analyze these streams efficiently — without writing full programs — is one of the most practically valuable skills in Linux administration and DevOps.

---

## 💡 Why These Three Tools?

Three tools have endured for decades because they each solve a distinct and common problem:

| Tool | Full Name | Primary Job | Core Strength |
| :---: | :--- | :--- | :--- |
| `grep` | **G**lobal **R**egular **E**xpression **P**rint | Search | Find matching lines |
| `sed` | **S**tream **Ed**itor | Edit / Transform | Substitute, insert, delete |
| `awk` | **A**ho, **W**einberger, **K**ernighan (authors) | Analyze / Report | Fields, calculations, reports |

---

## 🧠 Mental Model

```text
Text / Command Output
        │
        ├──────────▶  grep  ──▶  FIND matching lines
        │
        ├──────────▶  sed   ──▶  CHANGE / TRANSFORM text
        │
        └──────────▶  awk   ──▶  ANALYZE / REPORT
                                   │
                                   ├──▶ FILTER rows by condition
                                   ├──▶ CALCULATE (sum, avg, max)
                                   └──▶ FORMAT / REPORT output
```

---

## 🔗 The Power of Pipes

Each tool reads from **standard input** and writes to **standard output**. This means they can be chained together using the **pipe operator** `|`:

```text
Input Source
    │
   grep    ◀── Filter: only keep relevant lines
    │
   sed     ◀── Transform: clean/change those lines
    │
   awk     ◀── Analyze: compute or format the result
    │
Final Output
```

A pipe `|` sends the **stdout** of one command to the **stdin** of the next. No temporary files are created — data flows as a stream.

---

## 📌 What Text Do These Tools Work On?

```text
✅ Application and system logs      (/var/log/syslog, app.log)
✅ Configuration files              (nginx.conf, /etc/hosts, .env)
✅ CSV / TSV structured data        (employees.txt, reports.csv)
✅ Command output                   (ps aux, df -h, ip addr)
✅ Process information              (top, journalctl -u nginx)
✅ Source code / Bash scripts       (grep for TODOs, sed for bulk replace)
```

---

## 🧭 Decision Guide

```text
┌────────────────────────────────────────────────────────┐
│  Question                   →  Tool                    │
├────────────────────────────────────────────────────────┤
│  "Does this line contain X?"  →  grep                  │
│  "Replace this text"          →  sed                   │
│  "Extract column 3"           →  awk                   │
│  "Sum a column of numbers"    →  awk                   │
│  "Delete comment lines"       →  sed  (or grep -v)     │
│  "Count lines matching X"     →  grep -c               │
│  "Group by dept, count rows"  →  awk                   │
│  "Find ERRORs, show 3 lines"  →  grep -C 3             │
│  "Find & replace in a file"   →  sed -i                │
│  "Top IPs in access.log"      →  awk | sort | uniq -c  │
└────────────────────────────────────────────────────────┘
```

---

## 📚 Learning Path for This Module

```text
01. Overview (this file)
02. Sample Data (employees.txt — used in all examples)
03. grep   — Search
04. sed    — Edit/Transform
05. awk    — Analyze/Report
06. Pipelines — Combining tools
07. Real-World DevOps Scenarios
08. Troubleshooting
09. Interview Q&A
10. Hands-On Lab
11. MCQs
12. Quick Revision
13. Related Topics
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Sample Data](./02-Sample-Data.md) |
