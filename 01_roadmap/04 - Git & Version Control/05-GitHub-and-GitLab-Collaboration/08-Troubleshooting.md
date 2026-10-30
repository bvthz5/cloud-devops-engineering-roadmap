# 08 - GitHub Collaboration: Troubleshooting Guide

## 1. Syncing a Diverged Local Branch with Remote

```bash
# Discard local unpushed commits and force match remote main:
git checkout main
git fetch origin
git reset --hard origin/main
```

---

## 2. Fixing "Merge Conflict in PR" Without GUI
```bash
git checkout feature/branch
git fetch origin
git rebase origin/main
# Fix conflict markers in editor
git add <resolved-files>
git rebase --continue
git push --force-with-lease origin feature/branch
```
*Note: Always use `--force-with-lease` instead of raw `--force` to ensure you don't overwrite teammates' commits.*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
