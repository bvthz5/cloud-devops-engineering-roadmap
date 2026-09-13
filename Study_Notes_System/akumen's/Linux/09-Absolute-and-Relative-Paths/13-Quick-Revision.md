# 13 - Quick Revision Cheat Sheet: Absolute & Relative Paths

A 5-minute high-density reference sheet for Linux path syntax, directory symbols, and wildcard expansion rules.

---

## 🚀 Quick Reference Matrix

| Concept | Syntax / Symbol | Meaning | Usage Example |
| :--- | :---: | :--- | :--- |
| **Absolute Path** | `/path/file` | Full path starting from Root (`/`). | `/var/log/syslog` |
| **Relative Path** | `path/file` | Path relative to `pwd`. | `projects/app.py` |
| **Current Directory** | `.` | Current Working Directory. | `./script.sh` or `cp /file .` |
| **Parent Directory** | `..` | Directory one level up. | `cd ..` or `../../etc` |
| **Home Directory** | `~` | Current user home (`$HOME`). | `cd ~` |
| **Previous Directory**| `-` | Last working directory (`$OLDPWD`). | `cd -` |
| **Zero+ Wildcard** | `*` | Matches 0 or more characters. | `rm *.tmp` |
| **Single Wildcard** | `?` | Matches exactly 1 character. | `ls log?.txt` |
| **Range Match** | `[a-z]` | Matches 1 character in range. | `ls file[1-5].txt` |
| **Brace Expansion** | `{a,b}` | Generates literal comma strings. | `mkdir -p {bin,src}` |

---

## ⬅️ Navigation
- Previous: [12 - Multiple Choice Questions](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/12-MCQ.md)
- Next: [14 - Related Topics & Next Steps](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/14-Related-Topics.md)
