# 09 — Linux Backup Interview Q&A

10 technical interview questions for DevOps, SRE, and Systems Engineering roles.

---

### Q1: How does rsync determine whether a file has changed?
**Answer:**
By default, rsync uses a fast "quick check" heuristic comparing the file's **size** and **last modification timestamp (mtime)**. If both match, rsync assumes the file is identical and skips it.
If the `-c` (`--checksum`) flag is specified, rsync reads both files and computes a 128-bit MD5 checksum on both ends. This is 100% accurate for detecting subtle file content changes with identical timestamps, but incurs higher disk I/O.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
