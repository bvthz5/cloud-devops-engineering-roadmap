# 11 - Hands-On Practice & Terminal Labs: Absolute & Relative Paths

Practical terminal labs to practice path navigation, relative traversals, and wildcard matching.

---

## 🧪 Lab 1: Relative Path Traversal Challenge

1. Open your terminal and create a practice directory tree:
   ```bash
   mkdir -p /tmp/path_lab/alpha/beta/gamma
   mkdir -p /tmp/path_lab/delta
   touch /tmp/path_lab/alpha/beta/gamma/target.txt
   ```
2. Navigate into `alpha/beta/gamma`:
   ```bash
   cd /tmp/path_lab/alpha/beta/gamma
   pwd
   ```
3. Copy `target.txt` to `delta` using **ONLY relative paths** (without using `/tmp` or `/` in your command):
   ```bash
   cp target.txt ../../../delta/
   ```
4. Verify the file exists in `delta`:
   ```bash
   ls -l ../../../delta/target.txt
   ```
5. Clean up the practice lab:
   ```bash
   rm -rf /tmp/path_lab
   ```

---

## 🧪 Lab 2: Wildcard Matching Exercises

1. Create test files in `/tmp`:
   ```bash
   touch /tmp/test_{1,2,3,4,5}.log
   touch /tmp/data_{a,b,c}.txt
   ```
2. List only log files numbered 1 through 3 using character range wildcards:
   ```bash
   ls /tmp/test_[1-3].log
   ```
3. Clean up test files:
   ```bash
   rm -f /tmp/test_*.log /tmp/data_*.txt
   ```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Interview QA](./10-Interview-QA.md) | [README](./README.md) | [12 - MCQ](./12-MCQ.md) |
