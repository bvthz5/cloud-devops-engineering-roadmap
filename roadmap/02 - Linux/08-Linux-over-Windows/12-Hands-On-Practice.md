# 12 - Hands-On Practice & Terminal Labs: Linux over Windows

Practical terminal exercises for cross-platform file conversions, line ending fixes, and resource efficiency comparisons.

---

## 🧪 Lab 1: Simulating and Fixing CRLF Line Ending Corruption

1. Create a script file with CRLF line endings using `printf`:
   ```bash
   printf '#!/bin/bash\r\necho "Testing line endings"\r\n' > crlf_script.sh
   chmod +x crlf_script.sh
   ```
2. Attempt to execute the script:
   ```bash
   ./crlf_script.sh
   ```
3. Inspect file format using `file` command:
   ```bash
   file crlf_script.sh
   # Output will report: CRLF line terminators
   ```
4. Convert line endings using `dos2unix` or `sed`:
   ```bash
   sed -i 's/\r$//' crlf_script.sh
   file crlf_script.sh
   ./crlf_script.sh
   ```

---

## 🧪 Lab 2: Comparing Memory Footprint with `free` and `ps`

1. Check system RAM usage in human-readable format:
   ```bash
   free -h
   ```
2. Calculate memory consumed by process page tables and buffers:
   ```bash
   cat /proc/meminfo | grep -iE 'MemTotal|MemFree|MemAvailable|Buffers|Cached'
   ```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Interview QA](./11-Interview-QA.md) | [README](./README.md) | [13 - MCQ](./13-MCQ.md) |
