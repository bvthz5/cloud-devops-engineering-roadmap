# 02 - Merge Strategies: Fast-Forward vs. 3-Way

## 1. Fast-Forward Merge

A **Fast-Forward Merge** occurs when the target branch (`main`) has had **zero new commits** since the feature branch diverged:

```
BEFORE MERGE:
main:     C1 ──► C2
                  └──► C3 ──► C4 (feature)

AFTER FAST-FORWARD MERGE (`git merge feature`):
main:     C1 ──► C2 ──► C3 ──► C4 (feature)
```
Git simply moves the `main` pointer forward to point at `C4`. No new merge commit is created!

---

## 2. 3-Way Merge (True Merge)

When both branches have progressed independently, Git finds the **Common Ancestor (Merge Base)** and executes a 3-Way Merge:

```
BEFORE MERGE:
          C1 ──► C2 ──► C5 (main)
                  └──► C3 ──► C4 (feature)

AFTER 3-WAY MERGE (`git merge feature`):
          C1 ──► C2 ──► C5 ──────► C6 (Merge Commit)
                  └──► C3 ──► C4 ──┘
```
- Git creates a new commit `C6` that has **two parent commits**: `C5` and `C4`.

---

## 3. Disabling Fast-Forward (`--no-ff`)
Many organizations enforce `--no-ff` on pull requests to preserve a clear historical record of when a feature was integrated:
```bash
git merge --no-ff feature
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Git Branch Mechanics Under the Hood](./01-Git-Branch-Mechanics-Under-the-Hood.md) | [Index](../../../README.md) | [03 - Git Rebase Mechanics and The Golden Rule →](./03-Git-Rebase-Mechanics-and-The-Golden-Rule.md) |
