# 08 - Git Workflows: Troubleshooting Guide

## 1. Untangling a Stale Feature Branch

If a branch has fallen 200 commits behind `main`:

```bash
# Step 1: Update main
git checkout main && git pull origin main

# Step 2: Rebase feature branch iteratively onto main
git checkout feature/old
git rebase main

# If conflict occurs:
# Resolve conflicting file, git add <file>, then:
git rebase --continue
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
