# 06 - Reverting Commits and 3-Way Merge Reversals

## 1. `git revert` vs. `git reset`

- **`git reset`:** Erases commits from history. **Dangerous on public/shared branches.**
- **`git revert`:** Creates a **brand-new commit** that introduces the exact inverse diff of the target commit. **Safe for production.**

```bash
# Revert a single commit
git revert 4f1a2b3
```

---

## 2. Reverting a 3-Way Merge Commit (`-m 1`)

If you attempt `git revert <merge-commit-hash>`, Git fails with:
```
fatal: commit <hash> is a merge but no -m option was given.
```

### Why?
A merge commit has **two parent commits**. Git does not know which parent branch should be retained as the baseline!

```bash
# -m 1 instructs Git to keep Parent 1 (the branch you merged INTO, typically main)
git revert -m 1 <merge-commit-hash>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - git bisect](./05-Automated-Bug-Hunting-with-git-bisect.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
