# 08 - Resolving Conflicts: Troubleshooting Guide

## 1. Clearing Stuck Git Rebase State

If a rebase becomes completely corrupted and `git rebase --abort` fails:

```bash
# Manually remove rebase state directories
rm -rf .git/rebase-apply .git/rebase-merge

# Reset working tree cleanly
git reset --hard HEAD
```

---

## 2. Resolving "Untracked working tree file would be overwritten by merge"
```bash
# Stash untracked files or clean them
git clean -fd
git pull
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
