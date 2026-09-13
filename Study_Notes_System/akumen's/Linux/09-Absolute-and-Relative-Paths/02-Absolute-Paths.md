# 02 - Absolute Paths

An **Absolute Path** is a complete, fully-qualified file specification that specifies a location starting from the **Root Directory (`/`)**.

---

## 📌 Characteristics of Absolute Paths

1. **Always Starts with `/`:** Every absolute path begins with a leading forward slash (e.g., `/var/log/nginx/access.log`).
2. **Context-Independent:** An absolute path points to the exact same file location regardless of what your current working directory (`pwd`) is.
3. **Deterministic:** No ambiguity. It provides a unique, non-changing reference across the system.

---

## 💡 Examples of Absolute Paths

- `/etc/passwd` -> The user account file inside `/etc`.
- `/var/log/syslog` -> The main system log file inside `/var/log`.
- `/home/ubuntu/app/main.py` -> A Python file inside user `ubuntu`'s home directory.
- `/usr/bin/python3` -> The Python 3 executable binary inside `/usr/bin`.

```bash
# Regardless of current directory (/home/alice, /tmp, or /var), this command always edits the same file:
nano /etc/nginx/nginx.conf
```

---

## ⚙️ Why Absolute Paths are Mandatory in DevOps Automation

In automated environments—such as **Cron jobs**, **Systemd service units**, **CI/CD pipeline scripts**, and **Docker volume mounts**—processes execute with non-standard or unpredictable working directories.

Using relative paths in cron scripts (e.g., `python3 script.py`) often results in `command not found` or `file not found` errors. 

**DevOps Golden Rule:** Always specify absolute paths in scripts and configuration files:
```bash
# BAD in Cron:
python3 script.py > log.txt

# GOOD in Cron:
/usr/bin/python3 /home/ubuntu/scripts/script.py > /var/log/script.log 2>&1
```

---

## ⬅️ Navigation
- Previous: [01 - Path Basics](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/01-Path-Basics-and-Mental-Model.md)
- Next: [03 - Relative Paths](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/03-Relative-Paths.md)
