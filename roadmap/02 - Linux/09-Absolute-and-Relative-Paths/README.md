# 09 - Absolute & Relative Paths in Linux

Welcome to the study module on **Absolute and Relative Paths in Linux**.

---

## 📌 Overview

Path navigation and file location resolution form the foundation of command-line operations, shell scripting, system administration, and automated deployment pipelines. In Linux, every file and directory lives inside a single unified tree starting at Root (`/`).

This module provides a comprehensive breakdown of **Absolute Paths** (full paths starting from `/`), **Relative Paths** (paths relative to the current working directory), special directory shortcuts (`.`, `..`, `~`), **Wildcards & Shell Globbing** (`*`, `?`, `[]`, `{}`), real-world DevOps scenarios, troubleshooting, interview prep, and hands-on terminal exercises.

---

## 🗺️ Architectural Path Resolution Diagram

```mermaid
graph TD
    Root["/ (Root Directory)"]
    Root --> Var["var/"]
    Root --> Home["home/"]
    Root --> Etc["etc/"]
    
    Var --> Log["log/"]
    Log --> Nginx["nginx/"]
    Nginx --> AccessLog["access.log"]
    
    Home --> User["ubuntu/"]
    User --> Projects["projects/"]
    Projects --> App["app.py"]
    
    classDef absPath fill:#1f77b4,stroke:#333,stroke-width:2px,color:#fff;
    classDef relPath fill:#2ca02c,stroke:#333,stroke-width:2px,color:#fff;
    
    click Root href "./02-Absolute-Paths.md"
```

```text
Working Directory: /home/ubuntu

Absolute Path to access.log:
  /var/log/nginx/access.log  (Starts from Root /)

Relative Path to access.log:
  ../../var/log/nginx/access.log (Navigates up 2 levels then down)

Relative Path to app.py:
  projects/app.py  (Starts from current directory /home/ubuntu)
```

---

## 📚 Module Breakdown

| # | File / Module | Key Focus Areas |
| :---: | :--- | :--- |
| **01** | [`01-Path-Basics-and-Mental-Model.md`](./01-Path-Basics-and-Mental-Model.md) | What is a path? The inverted tree model and current working directory (`pwd`) |
| **02** | [`02-Absolute-Paths.md`](./02-Absolute-Paths.md) | Full paths starting at `/`, determinism, system scripts, and cron safety |
| **03** | [`03-Relative-Paths.md`](./03-Relative-Paths.md) | Context-dependent paths, interactive terminal convenience, portable code |
| **04** | [`04-Dot-and-DotDot-Mechanics.md`](./04-Dot-and-DotDot-Mechanics.md) | The current directory (`.`), parent directory (`..`), and home (`~`, `-`) shortcuts |
| **05** | [`05-Absolute-vs-Relative-Comparison.md`](./05-Absolute-vs-Relative-Comparison.md) | Detailed side-by-side comparison matrix and selection guidelines |
| **06** | [`06-Wildcards-and-Shell-Globbing.md`](./06-Wildcards-and-Shell-Globbing.md) | Shell wildcards (`*`, `?`, `[a-z]`, `{1..5}`) and path expansion mechanics |
| **07** | [`07-Practical-Command-Examples.md`](./07-Practical-Command-Examples.md) | Real-world CLI examples with `cd`, `ls`, `cp`, `mv`, `find`, `ln` |
| **08** | [`08-Real-World-Production-Scenarios.md`](./08-Real-World-Production-Scenarios.md) | Production pitfalls: Cron failures, broken symlinks, Docker bind mounts |
| **09** | [`09-Troubleshooting.md`](./09-Troubleshooting.md) | Diagnosing `No such file or directory` & path resolution errors |
| **10** | [`10-Interview-QA.md`](./10-Interview-QA.md) | Technical interview Q&A on path resolution and shell expansion |
| **11** | [`11-Hands-On-Practice.md`](./11-Hands-On-Practice.md) | Practical terminal labs and wildcard challenges |
| **12** | [`12-MCQ.md`](./12-MCQ.md) | Self-assessment multiple-choice quiz |
| **13** | [`13-Quick-Revision.md`](./13-Quick-Revision.md) | 5-minute high-density revision cheat sheet |
| **14** | [`14-Related-Topics.md`](./14-Related-Topics.md) | Next steps in Linux navigation & shell scripting |
| **SOURCE** | [`SOURCE.md`](./SOURCE.md) | Source attribution & educational expansion notes |

---

## 🎯 Learning Objectives

By completing this module, you will understand:
1. The exact mechanism of path resolution starting from Root (`/`) vs Current Working Directory (`pwd`).
2. Why automated system scripts (Cron jobs, Systemd services) must always use Absolute Paths.
3. How special path shortcuts (`.`, `..`, `~`, `-`) function at the filesystem level.
4. How the shell expands wildcards (`*`, `?`, `[0-9]`) before handing arguments to binary executables.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| — | You are here | [01 - Path Basics and Mental Model](./01-Path-Basics-and-Mental-Model.md) |
