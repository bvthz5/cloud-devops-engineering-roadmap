# 05 - Absolute vs. Relative Comparison Matrix

A detailed comparison between Absolute and Relative Paths across operational dimensions.

---

## 📊 Comparison Matrix

| Operational Dimension | Absolute Path | Relative Path |
| :--- | :--- | :--- |
| **Starting Point** | Always starts from Root (`/`). | Starts from Current Working Directory (`pwd`). |
| **Leading Character** | Always begins with `/`. | Begins with filename, `.`, `..`, or subdirectory. |
| **Dependence on `pwd`** | **Independent.** Resolves identically regardless of `pwd`. | **Dependent.** Resolves differently if `pwd` changes. |
| **Primary Use Cases** | System scripts, Cron jobs, Systemd units, Docker mounts. | Interactive CLI navigation, portable project code repos. |
| **Portability** | Low portability across different host environments. | High portability within project directory structures. |
| **Length & Brevity** | Verbose and long (`/home/ubuntu/app/config.json`). | Concise and short (`config.json`). |
| **Risk in Scripts** | Safe. Eliminates execution ambiguity. | Risky. Fails if script runs from an unexpected `pwd`. |

---

## 💡 Selection Decision Flowchart

```text
Is the path used in an automated system script (Cron, Systemd, CI/CD)?
├── YES ➔ Use ABSOLUTE PATH (/var/log/app.log)
└── NO
    └── Is it interactive CLI typing or portable project code?
        ├── YES ➔ Use RELATIVE PATH (../data/file.csv)
        └── NO  ➔ Default to ABSOLUTE PATH for safety
```

---

## ⬅️ Navigation
- Previous: [04 - Special Path Shortcuts](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/04-Dot-and-DotDot-Mechanics.md)
- Next: [06 - Wildcards & Shell Globbing](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/06-Wildcards-and-Shell-Globbing.md)
