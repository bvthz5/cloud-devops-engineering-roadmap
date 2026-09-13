# 06 - Wildcards & Shell Globbing

**Wildcards** (also known as **Glob patterns**) are special characters used by the Linux shell interpreter (Bash/Zsh) to perform pattern matching across filenames and directory paths.

---

## ⚙️ How Shell Globbing Works

When you type a command containing wildcards (e.g., `ls *.log`), the **shell interpreter expands the wildcard pattern into matching file paths BEFORE invoking the target binary**.

```text
User Input:
  ls *.txt

1. Shell Glob Expansion:
   Looks in current directory -> finds 'file1.txt', 'file2.txt'

2. Binary Invocation:
   execve("/usr/bin/ls", ["ls", "file1.txt", "file2.txt"])
```

---

## 🔣 Core Wildcard Operators

| Wildcard Pattern | Description | Matching Examples |
| :---: | :--- | :--- |
| **`*`** | Matches **zero or more** arbitrary characters. | `*.log` matches `syslog`, `access.log`, `.log` |
| **`?`** | Matches **exactly one** single character. | `file?.txt` matches `file1.txt`, `fileA.txt` |
| **`[characters]`** | Matches **any single character** inside the brackets. | `file[123].txt` matches `file1.txt`, `file2.txt`, `file3.txt` |
| **`[a-z]` / `[0-9]`** | Matches **any single character** within the range. | `log[0-9].txt` matches `log1.txt` through `log9.txt` |
| **`[!characters]`** | Matches **any single character NOT** in the brackets. | `file[!0-9].txt` matches `fileA.txt` (not numbers) |
| **`{str1,str2}`** | Brace expansion (generates combinations). | `file.{txt,pdf,png}` expands to 3 separate filenames |

---

## 💡 Practical Examples of Shell Globbing

```bash
# Delete all .tmp files in current directory
rm *.tmp

# List all 4-letter log files starting with 'app'
ls app?.log

# Move all log files starting with 2023 or 2024 to archive/
mv log_202[34]*.log archive/

# Create multiple directories at once using brace expansion
mkdir -p project/{src,bin,docs,tests}
```

---

## ⬅️ Navigation
- Previous: [05 - Absolute vs Relative Comparison](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/05-Absolute-vs-Relative-Comparison.md)
- Next: [07 - Practical Command Examples](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/07-Practical-Command-Examples.md)
