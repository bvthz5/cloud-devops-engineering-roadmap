# 03 - Git Rebase Mechanics and The Golden Rule

## 1. How Rebase Works

Rebasing **replays** commits from your branch on top of another base tip, creating a clean, perfectly linear project history:

```
BEFORE REBASE:
main:     C1 ──► C2 ──► C5
                  └──► C3 ──► C4 (feature)

DURING REBASE (`git rebase main` on feature branch):
1. Git saves C3 and C4 as temporary patches in .git/rebase-apply.
2. Git resets feature branch to match C5 (tip of main).
3. Git applies C3 onto C5 -> Creates C3' (new hash!).
4. Git applies C4 onto C3' -> Creates C4' (new hash!).

AFTER REBASE:
main:     C1 ──► C2 ──► C5
                         └──► C3' ──► C4' (feature)
```

---

## 2. The Golden Rule of Rebasing

> **NEVER rebase commits that exist outside your local repository and have been shared with other people on a public/main branch!**

### Why?
Rebasing replaces existing commits with **completely new commit hashes**. If teammates have based their work on your original commit `C3`, and you force-push rebased `C3'`, their local history is shattered, leading to duplicated commits and merge chaos.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Merge Strategies](./02-Merge-Strategies-Fast-Forward-vs-3-Way.md) | [README](./README.md) | [04 - Interactive Rebasing](./04-Interactive-Rebasing-Squash-Fixup-and-Edit.md) |
