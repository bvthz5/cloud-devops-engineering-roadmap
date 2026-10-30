# 08 - Branching & Merging: Troubleshooting Guide

## 1. Aborting Rebase or Merge Safely

If a complex rebase or merge goes wrong and produces unexpected conflicts, return instantly to your original state:

```bash
# Abort an ongoing merge and reset working tree
git merge --abort

# Abort an ongoing rebase and return branch pointer to pre-rebase state
git rebase --abort

# Abort a cherry-pick
git cherry-pick --abort
```

---

## 2. Recovering a Deleted Branch
If you accidentally ran `git branch -D feature-login`:
```bash
# Find the commit SHA where the branch tip was
git reflog
# Output: 4f1a92e HEAD@{2}: commit: finalize login controller

# Re-create the branch at that exact commit
git branch feature-login 4f1a92e
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
