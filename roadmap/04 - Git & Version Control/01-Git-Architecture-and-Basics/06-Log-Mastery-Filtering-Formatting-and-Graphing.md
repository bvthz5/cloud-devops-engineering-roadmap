# 06 - Log Mastery: Filtering, Formatting, and Graphing

## 1. Professional Git Log Formats

The default `git log` output is verbose and clutters the terminal. Professional engineers customize formatting for fast scanning:

```bash
# High-density oneline graph of history across all branches
git log --graph --oneline --decorate --all

# Custom enterprise pretty format
git log --graph --pretty=format:'%C(yellow)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```

---

## 2. Powerful Filtering Options

```bash
# Filter commits by Author
git log --author="Alice"

# Filter commits within a specific date range
git log --since="2026-09-01" --until="2026-10-01"

# Filter commits by commit message keyword (Regex)
git log --grep="CVE-2026"

# Trace history of changes to a specific file
git log -p path/to/Dockerfile

# "Pickaxe" search (-S): Find commits that introduced or deleted a specific string
git log -S "AWS_SECRET_ACCESS_KEY"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Diffing and State Inspection Working vs Staged](./05-Diffing-and-State-Inspection-Working-vs-Staged.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
