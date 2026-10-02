# 08 - Git Architecture: Troubleshooting Guide

## 1. Quick Diagnostic Decision Tree

```
Accidental action or state?
  │
  ├── 1. Staged a file accidentally (Need to unstage)
  │      └── git restore --staged <file>
  │
  ├── 2. Modified a file on disk (Want to discard local edits)
  │      └── git restore <file>
  │
  ├── 3. Committed with wrong message or author email
  │      └── git commit --amend --author="Name <email>"
  │
  └── 4. Completely lost a commit or HEAD moved
         └── git reflog (Find SHA) -> git reset --hard <SHA>
```

---

## 2. Essential Rescue Commands

### Safely Unstaging Changes
```bash
# In Git 2.23+:
git restore --staged sensitive_config.json

# In legacy Git:
git reset HEAD sensitive_config.json
```

### Amending the Most Recent Commit
```bash
# Add a forgotten file to the previous commit without creating a new commit
git add forgotten_file.go
git commit --amend --no-edit
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
