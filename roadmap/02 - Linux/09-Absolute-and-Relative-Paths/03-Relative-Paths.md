# 03 - Relative Paths

A **Relative Path** specifies a file or directory location **relative to your Current Working Directory (`pwd`)**.

---

## 📌 Characteristics of Relative Paths

1. **Never Starts with `/`:** A relative path begins with a file/folder name, a dot (`.`), or a double dot (`..`).
2. **Context-Dependent:** The target location resolved by a relative path changes depending on what your current working directory (`pwd`) is when the command is run.
3. **Shorter for Interactive Use:** Reduces typing during interactive terminal navigation.

---

## 💡 Examples of Relative Paths

Suppose your Current Working Directory (`pwd`) is **/home/ubuntu**:

| Target Location | Absolute Path | Relative Path from `/home/ubuntu` |
| :--- | :--- | :--- |
| File in current folder | `/home/ubuntu/notes.txt` | `notes.txt` or `./notes.txt` |
| Subdirectory file | `/home/ubuntu/projects/app.py` | `projects/app.py` |
| Parent directory file | `/home/notes.txt` | `../notes.txt` |
| File in separate tree | `/var/log/syslog` | `../../var/log/syslog` |

---

## 🚀 When to Use Relative Paths

- **Interactive Terminal Operations:** Quick file creation, moving, and viewing (e.g., `cd projects`, `cat README.md`).
- **Portable Code Repositories:** Referencing project assets inside Git repositories (`import ./utils/helper.py`). Portable code will run on any workstation regardless of whether the repo is cloned to `/home/user/repo` or `/opt/repo`.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Absolute Paths](./02-Absolute-Paths.md) | [README](./README.md) | [04 - Dot and DotDot Mechanics](./04-Dot-and-DotDot-Mechanics.md) |
